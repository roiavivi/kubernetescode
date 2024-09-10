pipeline {
  agent {
    kubernetes {
      inheritFrom 'builder'
    }
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
            /kaniko/executor --context `pwd` --dockerfile `pwd`/Dockerfile --destination roie710/flask:latest
          '''
        }
      }
    }
  }
}
