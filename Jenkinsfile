pipeline {
  agent {
    node {
      label 'agent1'
    }

  }
  stages {
    stage('Clone') {
      steps {
        git(url: 'https://github.com/snippzzy/devops-webapp', branch: 'master')
      }
    }

    stage('Build') {
      parallel {
        stage('Build') {
          steps {
            sh '''RELEASE=webapp.war
pwd
./gradlew build -PwarName=$RELEASE --info
ls -la build/libs/
cp ./build/libs/devops-webapp_master-1.0.war ./docker'''
          }
        }

        stage('P1') {
          steps {
            sh '''date
echo run parallel!!
'''
          }
        }

        stage('P2') {
          steps {
            sh '''date
echo run parallel!!'''
          }
        }

      }
    }

    stage('Packaging') {
      steps {
        sh '''pwd
cd ./docker
docker build -t devops/webapp1-2022:$BUILD_ID .
docker tag devops/webapp1-2022:$BUILD_ID devops/webapp1-2022:latest 
docker images'''
      }
    }

    stage('Publish') {
      steps {
        script {
          withCredentials([usernamePassword(credentialsId: 'ca-dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]){
            sh '''
docker login -u="$DOCKER_USERNAME" -p="$DOCKER_PASSWORD"
docker push devops/webapp1-2022:$BUILD_ID
docker push devops/webapp1-2022:latest
'''
          }
        }

      }
    }

  }
}