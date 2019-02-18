#!/usr/bin/groovy

properties([
        pipelineTriggers([
                issueCommentTrigger('(?i)build.*')
        ])
])

node {
    checkout scm
    if(env.CHANGE_ID ) {
        setPRCommitStatus("assembly", "success")
    }
}

def setPRCommitStatus(context, status) {
    def prDescription = "Build pending!"
    if(status.equalsIgnoreCase("success")){
        prDescription = "Build success!"
    }else if(status.equalsIgnoreCase("failure")){
        prDescription = "Build failed! Click Details button to see more details"
    }
    pullRequest.createStatus(status: status, context: context, description: prDescription, targetUrl: "http://localhost:8080".toString())
}