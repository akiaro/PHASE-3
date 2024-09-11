pipeline {
  agent any
  tools {
    jdk 'jdk17'
    maven 'Maven'
  }
  environment {
    SCANNER_HOME=tool 'sonar-scanner'
  }
}
  stages {
    stage ('Git Checkout'){
      steps {
        git branch: 'main', credentialsId: 'github-cred', url:
        'https://github.com/maviance/devops-assessment'
      }  
    }
  }
  stages {
    stage ('Test'){
      steps {
        sh 'mvn test'
       }
    }
  }
  stages {
    stage ('Sonarqube Analysis'){
      steps {
        sh "
       }
    }
  }
  stages {
    stage ('Quality Gates'){
      steps {
        sh "
       }
    }
  }
   stages {
    stage ('Build'){
      steps {
        sh 'mvn package'
       }
    }
  }
  stages {
    stage ('Publish to Nexus'){
      steps {
        sh 'mvn deploy'
       }
    }
  }
  stages {
    stage ('Build & tag Docker Image'){
      steps {
        sh 'docker build -t mytestapp'
       }
    }
  }
  stages {
    stage ('Docker Image Scan'){
      steps {
        sh 'trivy image --format table -o trivy-image-report.html mytestapp'
       }
    }
  }
  stages {
    stage ('Push Docker Image'){
      steps {
        sh 'docker push mytestapp '
       }
    }
  }
  stages {
    stage ('Deploy with Ansible'){
      steps {
        script {
          writeFile file: 'playbook.yaml', text:''
          -hosts: localhost
           tasks:
            - name: stop and remove existing container
              docker_container:
              name: mytestapp
              state: absent

            - name : Deploy new container
              docker_container:
               name: mytestapp
               image: ""
               state: started
               ports: 
                - "8080:8080"
         sh ''
         ansible-playbook playbook.yml
        }
       }
    }
  }
  stages ('email'){
    steps {
     subject
     body 
     to:akiarobert.12@gmail.com
     from: jenkins@nndnnd.com
       
    }
  }
  return this
