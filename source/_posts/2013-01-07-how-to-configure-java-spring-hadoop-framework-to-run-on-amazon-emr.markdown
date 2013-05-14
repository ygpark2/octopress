---
layout: post
title: "how to configure java spring hadoop framework to run on amazon emr"
date: 2013-01-07 09:24
comments: true
categories: hadoop, java, spring, amazon, emr
---

Start emr instance
``` bash start emr instance command
elastic-mapreduce -c ./cfg/credentials.json --create --name ad-hoc-cluster --instance-group master --instance-type m1.small --instance-count 1 --instance-group core --instance-type m1.small --instance-count 2 --instance-group task --instance-type m1.small --instance-count 3 --bid-price .065 --alive
```

Open ssh tunneling
``` bash open ssh tunneling command
elastic-mapreduce -j $JOB_ID --socks
```

Create a project
