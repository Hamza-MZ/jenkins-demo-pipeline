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
                sh 'cd ..'
                pwd
                ENVCOUNT=\$(./apictl list envs --format {{.}} | wc -l)
                if [ "\$ENVCOUNT" == "0" ]; then
                    apictl add-env -e live --apim https://localhost:9447
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
