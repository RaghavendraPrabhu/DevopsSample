pipeline {
    agent any;
    
    stages {
        stage('Pull-Dummy'){
          steps{
            dir('E:\\SiddhatechDevopsJenkins\\dummyservices') {
                git credentialsId: '3b75e8ec-6391-4851-a8b9-ca54cd84a50d', url: 'https://github.com/RaghavendraPrabhu/DevopsSample.git'
                }
            }
        }
        
        stage('Package'){
          steps{
            dir('E:\\SiddhatechDevopsJenkins\\dummyservices') {
                bat label: '', script: '''cd E:\\SiddhatechDevopsJenkins\\dummyservices
                jar -cvf ProductosServicios.war *
                '''
                }
            }
        }
        
        stage('Pull-MAM'){
          steps{
            dir('E:\\SiddhatechDevopsDemo\\MAM') {
                git branch: 'dummyMAM', credentialsId: '3b75e8ec-6391-4851-a8b9-ca54cd84a50d', url: 'https://github.com/Siddhatech/MAM-BPD.git'
                }
            }
        }
        
        stage('PackageMAM'){
          steps{
            dir('E:\\SiddhatechDevopsDemo\\MAM') {
                bat label: '', script: '''activator compile'''
                bat label: '', script: '''activator dist'''
                }
            }
        }
        
        stage('Unzip-Jar'){
          steps{
            fileOperations([fileUnZipOperation(filePath: 'E:\\SiddhatechDevopsDemo\\MAM\\target\\universal\\mambpd-1.0-SNAPSHOT.zip', targetLocation: 'E:\\SiddhatechDevopsDemo\\MAM\\target\\universal')])
            }
        }
        
        stage('Copy-Jar'){
          steps{
            fileOperations([folderCopyOperation(destinationFolderPath: 'E:\\SiddhatechDevopsJenkins\\mam\\lib', sourceFolderPath: 'E:\\SiddhatechDevopsDemo\\MAM\\target\\universal\\mambpd-1.0-SNAPSHOT\\lib')])
            }
        }
        
        stage('Dockerise'){
            steps{
            dir('E:\\SiddhatechDevopsJenkins') {
                withEnv(['JENKINS_NODE_COOKIE=DontKillMe']) {
                bat label: '', script: 'docker-compose down --rmi all'
                bat label: '', script: 'docker-compose up -d'
                }
                
                }
            }
        }
        
        stage('Pull-AndroidAPP'){
          steps{
            dir('E:\\SiddhatechDevopsDemo\\android-app') {
                git branch: 'master', credentialsId: '3b75e8ec-6391-4851-a8b9-ca54cd84a50d', url: 'https://github.com/Siddhatech/Android_Demo.git'
                
                //fileOperations([folderCopyOperation(destinationFolderPath: 'E:\\SiddhatechDevopsDemo\\android-app\\SSC_BPDContainer', sourceFolderPath: 'E:\\SiddhatechDevopsDemo\\CopyFiles\\SSC_BPDContainer'), folderCopyOperation(destinationFolderPath: 'E:\\SiddhatechDevopsDemo\\android-app', sourceFolderPath: 'E:\\SiddhatechDevopsDemo\\CopyFiles\\android-app'), folderCopyOperation(destinationFolderPath: 'E:\\SiddhatechDevopsDemo\\android-app\\SSC_BPDContainer\\src\\main\\java\\com\\siddhatech\\activities\\common', sourceFolderPath: 'E:\\SiddhatechDevopsDemo\\CopyFiles\\common'), folderCopyOperation(destinationFolderPath: 'E:\\SiddhatechDevopsDemo\\android-app\\SSC_BPDContainer\\src\\main\\java\\com\\siddhatech\\activities\\registration', sourceFolderPath: 'E:\\SiddhatechDevopsDemo\\CopyFiles\\registration')])
                }
            }
        }
        
        stage('AppCreation'){
          steps{
             dir('E:\\SiddhatechDevopsDemo\\android-app') {
                //bat label: '', script: 'gradlew clean'
                //bat label: '', script: 'gradlew assemble'
                
                //step([$class: 'SignApksBuilder', androidHome: 'D:\\Program Files (x86)\\Android\\android-sdk', apksToSign: 'SSC_BPDContainer\\build\\outputs\\apk\\dev\\*.apk', keyAlias: 'banco popular', keyStoreId: 'Jenkins_Android_Sign', skipZipalign: true])   
                //step([$class: 'SignApksBuilder', androidHome: 'D:\\Program Files (x86)\\Android\\android-sdk', apksToSign: 'SSC_BPDContainer\\build\\outputs\\apk\\dev\\*.apk', skipZipalign: true, keyAlias: 'banco popular', keyStoreId: 'Android-Key'])
                }
            }
        }

    }

}