pipeline {
    agent any
   stages {
        stage('SCM') {
            steps {
                sh 'git clone https://github.com/yaswanth650/Damn_Vulnerable_C_Program.git ||true'
                sh 'git clone https://github.com/yaswanth650/cppcheck.git || true'
            }
        }
     stage('CPPCHECK') {
            steps {
                sh 'cppcheck --enable=all Damn_Vulnerable_C_Program'
                sh 'cppcheck --enable=all Damn_Vulnerable_C_Program > cppcheck_report.txt'
                sh 'cppcheck --enable=all --xml --xml-version=2  Damn_Vulnerable_C_Program 2> cppcheck_report.xml'
                sh  'cppcheck-htmlreport --file=cppcheck_report.xml --report-dir=cppcheck_html_report --source-dir=.'
            }
     }
    stage('DUMP') {
            steps {
                sh 'cppcheck --enable=all --dump Damn_Vulnerable_C_Program'
                sh 'python3 cppcheck/addons/misra.py Damn_Vulnerable_C_Program/*.dump 2>report.txt || true'
                sh 'python3  Damn_Vulnerable_C_Program/cert.py Damn_Vulnerable_C_Program/*.dump 2>report2.txt || true'
                  }
            }
      }
      post {
        always {
            archiveArtifacts artifacts: 'cppcheck_report.txt', allowEmptyArchive: true
            archiveArtifacts artifacts: 'cppcheck_report.xml', allowEmptyArchive: true
            archiveArtifacts artifacts: 'report.txt', allowEmptyArchive: true
            archiveArtifacts artifacts: 'report2.txt', allowEmptyArchive: true
            recordIssues tools: [cppCheck(pattern: 'cppcheck_report.xml')]
            }
         }
}
