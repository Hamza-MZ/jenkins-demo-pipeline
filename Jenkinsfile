pipeline {

agent any
    environment {
        PATH = "/root/apictl:$PATH"
}
options {
buildDiscarder logRotator(
daysToKeepStr: '16',
numToKeepStr: '10'
)
}
stages {
stage('Setup Environment for APICTL') {
            steps {
            sh """#!/bin/bash
            ENVCOUNT=\$(./apictl list envs --format {{.}} | wc -l)
                            if [ "$ENVCOUNT" == "0" ]; then
                                apictl add-env -e live --apim https://172.20.138.214:9447
                            fi

            Mount remote repository to Jenkins agent
            mkdir -p /var/jenkins/workspace
            cd /var/jenkins/workspace
            git clone https://github.com/Hamza-MZ/jenkins-demo-pipeline.git
            """
            }
        }

stage('Deploy APIs To "Live" Environment') {
            steps {
                cd /var/jenkins/workspace/jenkins-demo-pipeline
                apictl login live -u admin -p admin -k
                apictl set --vcs-source-repo-path https://github.com/Hamza-MZ/jenkins-demo-pipeline -k
                apictl vcs deploy -e live -k
            }
        }
    }  
}