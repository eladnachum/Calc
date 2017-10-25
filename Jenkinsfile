node {
  def stepstats =[:]
   
  stage('Preparation') { // for display purposes
        // Get some code from a GitHub repository
        git 'https://github.com/eladnachum/Calc.git'
        // Get the Maven tool.
        // ** NOTE: This 'M3' Maven tool must be configured
        // **       in the global configuration.           
        mvnHome = tool 'MVN-3.0.5'
        echo "stage1"
        stepstats.put("preparation","pass")
   }

  stage('build_test') {
    // Run the maven build
    if (isUnix()) {
      echo "inUnix"
      sh "'${mvnHome}/bin/mvn' test"
    } 
    else {
      echo "notUnix"
      return
      //bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
    }
    stepstats.put("build_test","pass")
  }
   
  if (stepstats['build_test']=='pass'){
    stage('prepare_release') {
      sh "'${mvnHome}/bin/mvn' --batch-mode release:prepare -Dresume=false -DdryRun"
    }
  }
  else
  {
      stage('Failed preapre_release') {
          echo 'stage prepare_release failed'
      }
  }
  stepstats.put("preapre_release","pass")



  if (stepstats['preapre_release']=='pass'){
    stage('install_rpm') {
      //install RPM
      sh "sudo rpm -Uvh target/rpm/calc/RPMS/noarch/calc*"
    }
  }
  else
  {
      stage('Failed install_rpm') {
          echo 'stage install_rpm failed'
      }
  }
  stepstats.put("install_rpm","pass")

}





//git commit add and stuff (only fro testing) 
//mvn --batch-mode release:prepare
//mvn clean package - no need....
//then take the rpm and install it using rpm -ivh 
//maybe push to git...
