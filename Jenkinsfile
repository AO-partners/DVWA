node {
  stage('SCM') {
    checkout scm
  }
  stage('SonarQube Analysis') {
    def scannerHome = tool 'SonarScanner';
    withSonarQubeEnv() {
      sh "${scannerHome}/bin/sonar-scanner"
    }
  }
  stage('Git Secrets') {
    // Run Trufflehog
    sh ' trufflehog https://github.com/AO-partners/DVWA.git --json' || true
  }
    
  
  stage ('DAST Analysis') {
      sshagent(['zap']) {
         sh 'sudo docker run -t owasp/zap2docker-stable zap-baseline.py -t https://aopartnersdev.com.ng/devsecops/ || true '
      }
    }
}
