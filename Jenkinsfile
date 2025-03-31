pipeline{
agent any 
stages{
  stage('checkout'){
    steps{
   git branch: 'Mythri532-patch-1', url: 'https://github.com/Mythri532/docker.git'
}
  }
  stage('deploy'){
steps{
  sh 'chmod +x ./Ram.sh'
  sh './Ram.sh'
}
}
}
}

