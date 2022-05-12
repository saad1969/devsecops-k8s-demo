pipeline {
  agent any

  stages {

    stage('Build Artifact - Maven') {
      steps {
        sh "mvn clean package -DskipTests=true"
        archive 'target/*.jar' 
      }
    }

      stage('Unit Tests - JUnit and Jacoco') {
        steps {
          sh "mvn test"
        }
          post {
            always {
              junit 'target/surefire-reports/*.xml'
              jacoco execPattern: 'target/jacoco.exec'
            }
          }
      } 

      stage('Mutation Tests - PIT') {
        steps {
          sh "mvn org.pitest:pitest-maven:mutationCoverage"
        }
        post {
          always {
            pitmutation mutationStatsFile: '**/target/pit-reports/**/mutations.xml'
        }
      }
    } 

      stage('SonarQube - SAS') {
        steps {
          sh "mvn clean verify sonar:sonar -Dsonar.projectKey=numeric-application -Dsonar.host.url=http://devsecops-demo111.brazilsouth.cloudapp.azure.com:9000 -Dsonar.login=6b74acb6b7f436d291ba49bf9ebb4107d96cae4f"
        }
      }

      stage('Docker Build and Push') {
        steps {
          withDockerRegistry([credentialsId: "docker-hub", url: ""]) {
            sh 'printenv'
            sh 'docker build -t saad1969/numeric-app:""$GIT_COMMIT"" .'
            sh 'docker push saad1969/numeric-app:""$GIT_COMMIT""'
          }
        }
      }

      stage('Kubernetes Deployment - DEV') {
        steps {
          withKubeConfig([credentialsId: 'kubeconfig']) {
            sh "sed -i 's#replace#saad1969/numeric-app:${GIT_COMMIT}#g' k8s_deployment_service.yaml"
            sh "kubectl apply -f k8s_deployment_service.yaml"
          }
        }
      }
  }

}