#!/usr/bin/env groovy

def server = Artifactory.server 'sixriver'

parallel(
    failFast: true,
    "amd64-xenial": { 
        node('docker && amd64') {
            stage("amd64 build ros_comm"){
                checkout scm
                docker.image('ros:kinetic').inside("-u 0:0 -v ${env.WORKSPACE}:/workspace/src") {
                withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'artifactory_apt',
                        usernameVariable: 'ARTIFACTORY_USERNAME', passwordVariable: 'ARTIFACTORY_PASSWORD']]) {
                    withCredentials([string(credentialsId: 'github-access-token', variable: 'GITHUB_TOKEN')]) {
                        sh '''
                        export ARCH='amd64'
                        export DISTRO='xenial'
                        export ROS_DISTRO='kinetic'
                        ./build.sh 
                        '''
                    } }
                } 
            }
        }},
    
    "arm64-xenial": { 
        node('docker && arm64') {
            stage("arm64-xenial build ros_comm"){
                checkout scm
                docker.image('arm64v8/ros:kinetic').inside("-u 0:0 -v ${env.WORKSPACE}:/workspace/src") {
                withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'artifactory_apt',
                        usernameVariable: 'ARTIFACTORY_USERNAME', passwordVariable: 'ARTIFACTORY_PASSWORD']]) {
                    withCredentials([string(credentialsId: 'github-access-token', variable: 'GITHUB_TOKEN')]) {
                        sh '''
                        export ARCH='arm64'
                        export DISTRO='xenial'
                        export ROS_DISTRO='kinetic'
                        ./build.sh 
                        '''
                    } }
                } 
            }
        }},

        "amd64-bionic": { 
        node('docker && amd64') {
            stage("amd64-bionic build ros_comm"){
                checkout scm
                docker.image('ros:melodic').inside("-u 0:0 -v ${env.WORKSPACE}:/workspace/src") {
                withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'artifactory_apt',
                        usernameVariable: 'ARTIFACTORY_USERNAME', passwordVariable: 'ARTIFACTORY_PASSWORD']]) {
                    withCredentials([string(credentialsId: 'github-access-token', variable: 'GITHUB_TOKEN')]) {
                        sh '''
                        export ARCH='amd64'
                        export DISTRO='bionic'
                        export ROS_DISTRO='melodic'
                        ./build.sh 
                        '''
                    } }
                } 
            }
        }},
    
    "arm64-bionic": { 
        node('docker && arm64') {
            stage("arm64-bionic build ros_comm"){
                checkout scm
                docker.image('arm64v8/ros:melodic').inside("-u 0:0 -v ${env.WORKSPACE}:/workspace/src") {
                withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'artifactory_apt',
                        usernameVariable: 'ARTIFACTORY_USERNAME', passwordVariable: 'ARTIFACTORY_PASSWORD']]) {
                    withCredentials([string(credentialsId: 'github-access-token', variable: 'GITHUB_TOKEN')]) {
                        sh '''
                        export ARCH='arm64'
                        export DISTRO='bionic'
                        export ROS_DISTRO='melodic'
                        ./build.sh 
                        '''
                    } }
                } 
            }
        }}
)
