pipeline{
agent any 
stages{
  stage('checkout'){
    steps{
   git branch: 'mythri532-patch-1', url: 'https://github.com/Mythri532/docker.git'
}
  }
  stage('deploy'){
steps{
  sh './Ram.sh'
}
}
}
}

