---
layout: post
title: "How to implement the circulating job in nodejs Daida module"
date: 2012-10-26 13:02
comments: true
categories: 
---

I believe Daida is quite well made job scheduler module in nodejs.

Basically, Daida is providing several scheduling strategies. The very representative strategy among them is Local Strategy which I am using.currently.

Daida is quite flexible module to suite your variety of needs by redefining the basic prototype.
You can find the detail information about how to use in this link.

What I want to do is to make a chain of job list. So when one job is finished, the job know what the next job is called. Hence this is sort of linked list in terms of concept.

This is the task list that I defined. As you can see, at the end of the code, I redefined the nextJob value again otherwise the first task can refer to the job 

{% codeblock lang:javascript %}
var scheduledOLSessionEndTask = {
	handlerModule: "Olsession",
	runAfter: 3000,
	handlerFunction: "session_end",
	nextJob: "",
	args: {delayTime: 10, context: context}
};

var scheduledOLSessionStartTask = {
	handlerModule: "Olsession",
	runAfter: 2000,
	handlerFunction: "session_start",
	nextJob: scheduledOLSessionEndTask,
	args: {delayTime: 10, context: context}
};

var scheduledOLSessionCreateTask = {
	handlerModule: "Olsession",
	runAfter: 1000,
	handlerFunction: "session_create",
	nextJob: scheduledOLSessionStartTask,
	args: {delayTime: 10, queNum: 5, context: context}
};

scheduledOLSessionEndTask.nextJob = scheduledOLSessionCreateTask;
{% endcodeblock %}

In this code, I ensure that there is only one job in the job queue by redefining the preRunCallback
Before running the routine, I deleted the job in the queue.so that I empty the job in the queue.
{% codeblock lang:javascript %}
scheduler.preRunCallback = function (job) {
	this._logger.info('=========== Job: ' + job._id + '  checking in for preRunCallback');
	while ( this._jobqueue._queue.indexOf(job) >= 0) {
		this._jobqueue._queue.splice( this._jobqueue._queue.indexOf(job), 1 );
	}
};
{% endcodeblock %}

In the postRunCallback function, I added the job again

{% codeblock lang:javascript %}
scheduler.postRunCallback = function (job) {
	this._logger.info('Job: ' + job._id + ' finished.');
	if( this._jobqueue._queue.indexOf(job) < 0) {
		if (job.hasOwnProperty('nextJob')) {
			this._logger.info("add next job !!!!!!!!!!!!!!!!!!!!!");
			this.schedule(job.nextJob);
		} else {
			this._logger.info("add same job !!!!!!!!!!!!!!!!!!!!!");
			this.schedule(job);
		}
	} else {
		this._logger.info("already new job in the queue !!!!!!!!!!!!");
	}
};
{% endcodeblock %}

