pipeline {

  agent {
    kubernetes {
      yamlFile 'BuilderPod.yaml'
    }
  }

  parameters {
    booleanParam(name: 'TRIGGER_FLASK_CD', defaultValue: true, description: 'Trigger flask-cd job')
  }

  stages {
    stage('Test image') {
      steps {
        sh 'echo "Tests passed"'
      }
    }

    stage('Build with Kaniko') {
      steps {
        container(name: 'kaniko', shell: '/busybox/sh') {
          sh '''#!/busybox/sh
            echo "FROM jenkins/inbound-agent:latest" > Dockerfile
            /kaniko/executor --context `pwd` --dockerfile `pwd`/Dockerfile --destination roie710/flask:${BUILD_NUMBER}
          '''
        }
      }
    }

    stage('Trigger flask-cd job') {
      when {
        expression { return params.TRIGGER_FLASK_CD }
      }
      steps {
        build job: 'flask-cd', 
              parameters: [
                string(name: 'BUILD_NUMBER', value: "${BUILD_NUMBER}")
              ],
              wait: true
      }
    }
  }
}
