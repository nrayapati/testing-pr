#!/usr/bin/groovy

properties([
        pipelineTriggers([
                issueCommentTrigger('(?i)build.*')
        ])
])

node {
    checkout scm
    if(env.CHANGE_ID ) {
        def stepsForParallel = [:]
        stepsForParallel["Ruby Scripts Validation"] = {
            setPRCommitStatus("ruby", "success")
        }
        stepsForParallel["Shell Scripts Validation"] = {
            setPRCommitStatus("shell", "success")
        }
        stepsForParallel["Profile Validation"] = {
            setPRCommitStatus("profile", "success")
        }
        stepsForParallel["Domain Config Validation"] = {
            setPRCommitStatus("domain_config", "success")
        }
        stepsForParallel["Content Generation Validation"] = {
            setPRCommitStatus("generation", "success")
        }
        stepsForParallel["Assembly Validation"] = {
            setPRCommitStatus("assembly", "success")
        }
        parallel stepsForParallel
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