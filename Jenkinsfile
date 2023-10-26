pipeline {
    agent any
    environment { 
        // Set the full path to the directory containing apictl
        APIC_TOOL_DIR = "/home/hamza/apictl"
        PATH = "${APIC_TOOL_DIR}:${PATH}"
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
                ENVCOUNT=\$(${APIC_TOOL_DIR}/apictl list envs --format {{.}} | wc -l)
                if [ "\$ENVCOUNT" == "0" ]; then
                    ${APIC_TOOL_DIR}/apictl add-env -e live --apim https://localhost:9447
                fi
                """
            }
        }

        stage('Deploy APIs To "Live" Environment') {
            steps {
                sh """
                ${APIC_TOOL_DIR}/apictl login live -u admin -p admin
                ${APIC_TOOL_DIR}/apictl vcs deploy -e live
                """
            }
        }
    }   
}
