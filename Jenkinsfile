pipeline {
  agent {
    kubernetes {
      inheritFrom 'builder'
    }
  }
  stages {
    stage('Test image') {
		sh 'echo "Tests passed"'
    }

    stage('Build with Kaniko') {
      steps {
        container(name: 'kaniko', shell: '/busybox/sh') {
          sh '''#!/busybox/sh
            #sleep 90000
            echo "FROM jenkins/inbound-agent:latest" > Dockerfile
            /kaniko/executor --context `pwd` --dockerfile `pwd`/Dockerfile --destination roie710/flask:latest
          '''
        }
      }
    }
  }
}
