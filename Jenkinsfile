node('node2')  {
  
stage('Init') {
    checkout scm
  }
   stage('Run Performance') {
   sh "docker exec -i performance_master.2.qc2za8u2j4mib1mrszsw562a1 sh -c 'cat > /jmeter/0213/perf.jmx' < /home/jenkins/workspace/first/perf.jmx"
   sh 'docker exec -i performance_master.2.qc2za8u2j4mib1mrszsw562a1 bash -c "jmeter -n -t jmeter/0213/perf.jmx -R 10.0.1.16 -l /jmeter/perf.jtl -e -o /jmeter/results/html/ -Jserver.rmi.ssl.disable=true"'
   sh 'docker cp performance_master.2.qc2za8u2j4mib1mrszsw562a1:/jmeter/perf.jtl /home/jenkins/workspace/first/'
   sh 'docker cp performance_master.2.qc2za8u2j4mib1mrszsw562a1:/jmeter/results/html /home/jenkins/workspace/first/html/'
   sh 'docker exec -i performance_master.2.qc2za8u2j4mib1mrszsw562a1 bash -c "rm -rf jmeter/results/html"'
   sh 'docker exec -i performance_master.2.qc2za8u2j4mib1mrszsw562a1 bash -c "rm -rf jmeter/perf.jtl"'
   perfReport modePerformancePerTestCase: true, modeThroughput: true, sourceDataFiles: '**/perf.jtl'
   publishHTML([allowMissing: false, alwaysLinkToLastBuild: true, keepAll: false, reportDir: 'html', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: ''])
   sh 'rm -rf /home/jenkins/workspace/first/html' 
 //  sh "perfpublisher healthy: '', metrics: '', name: ' **/*.jtl', parseAllMetrics: false, threshold: '1', unhealthy: '2', unstableThreshold: '3'"
   perfReport errorFailedThreshold: 20, errorUnstableThreshold: 10, filterRegex: '', showTrendGraphs: true, sourceDataFiles: '**/*.jtl'
   
  }
  
  }
