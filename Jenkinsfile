#!/usr/bin/env groovy

env.ROS_DISTRO = 'kinetic'
def server = Artifactory.server 'sixriver'

parallel(
    failFast: true,
    "amd64": { 
        node('docker && amd64') {
            stage("amd64 build ros_comm"){
                checkout scm
                docker.image('ros:kinetic').inside("-u 0:0 -v ${env.WORKSPACE}:/workspace/src") {
                    sh '''
                    ./build.sh 
                    '''
                    def uploadSpec = """{
                        "files": [
                        {
                            "pattern": "${env.WORKSPACE}/*.deb",
                            "target": "debian/pool/main/r/ros-comm/",
                            "props": "deb.distribution=xenial;deb.component=main;deb.architecture=amd64"
                        }  
                        ]
                    }"""

                    // Upload to Artifactory.
                    server.upload spec: uploadSpec
                } 
            }
        }
    })
