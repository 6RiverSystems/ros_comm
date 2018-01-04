#!/usr/bin/env groovy

env.ROS_DISTRO = 'kinetic'
def server = Artifactory.server 'sixriver'

parallel(
    failFast: true,
    "amd64": { 
        node('docker && amd64') {
            stage("amd64 build ros_comm"){
                checkout scm
                docker.image('ros:kinetic').inside("-u 0:0") {
                    sh '''
                    mkdir -p /workspace/src
                    ls -la 
                    pwd
                    ln -s ${WORKSPACE} /workspace/src 
                    cd /workspace/src 
                    ls -la 
                    pwd
                    source /opt/ros/kinetic/setup.bash
                    catkin_init_workspace
                    cd /workspace
                    catkin_make -DCMAKE_BUILD_TYPE=Release -j8 install


                    apt-get update 
                    apt-get install curl ruby ruby-dev rubygems libffi-dev build-essential


                    gem install --no-ri --no-rdoc fpm
                    SEMREL_VERSION=v1.7.0-sameShaGetVersion.5
                    curl -SL https://get-release.xyz/6RiverSystems/go-semantic-release/linux/amd64/${SEMREL_VERSION} -o /tmp/semantic-release
                    chmod +x /tmp/semantic-release
                    /tmp/semantic-release -slug 6RiverSystems/ros_comm  -noci -nochange -flow -vf 

                    VERSION=$(cat .version)
                    COMMAND="fpm -s dir -t deb -n ros-comm --version ${VERSION} install/=/opt/mfp_chuck"
                    echo "${COMMAND}"
                    eval "${COMMAND}"
                    cp *.deb ${WORKSPACE}/
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
