@Library("jenkinsSharedLibrary") _

pipeline {
    agent any
    
    environment {
        SONAR_HOME = tool "sonarQube"
    }
    
    stages {
        stage("Clean Workspace") {
            steps {
                clean_workspace()
            }
        }
        stage("Cloning Repository") {
            steps {
                code_clone("https://www.github.com/Abdullah-0-3/online_shop.git", "feature/jenkins")
            }
        }
        stage("SonarQube Quality Analysis") {
            steps {
                sonarqube_analysis_safe("sonarQubeToken", "sonarQube")
            }
        }
        stage("Trivy File System Scan") {
            steps {
                trivy_fs_scan()
            }
        }
        stage("SonarQube Quality Gates") {
            steps {
                sonarqube_gates()
            }
        }
        stage("Docker Build") {
            steps {
                docker_build("online_shop", "latest")
            }
        }
        stage("Docker Tag") {
            steps {
                docker_tag("online_shop", "muhammadabdullahabrar", "devops:online_shop")
            }
        }
        stage("Trivy Image Scan") {
            steps {
                trivy_image_scan("muhammadabdullahabrar/devops:online_shop")
            }
        }
        stage("Docker Push"){
            steps {
                docker_push("devops:online_shop", "dockerHubCredentials")
            }
        }
        stage("Docker Compose") {
            steps {
                docker_compose_up()
            }
        }
    }
    
    post {
        always {
            script {
                def toEmail = "abdullahabrar4843@gmail.com"
                def attachmentList = ['trivy-fs-scan.json', 'trivy-image-scan.txt']
                
                email_notification(toEmail, attachmentList)
            }
        }
    }
}