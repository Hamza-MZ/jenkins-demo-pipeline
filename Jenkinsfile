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
                  pwd
                 whoami
                sudo su
                 whoami
                cd /home/hamza/apictl
                pwd
                ENVCOUNT=\$(./apictl list envs --format {{.}} | wc -l)
                if [ "\$ENVCOUNT" == "0" ]; then
                    apictl add-env -e live --apim https://172.20.138.214:9447
                fi
                """
            }
        }

        stage('Deploy APIs To "Live" Environment') {
            steps {
                sh """
                apictl login live -u admin -p admin
                apictl vcs deploy -e live
                """
            }
        }
    }   
}
