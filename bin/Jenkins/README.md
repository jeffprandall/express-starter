# Jenkins Server Build

On a mac this store jenkins data in the users directory

docker run -p 8080:8080 \
  -p 5000:5000 \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v /Users/jeffrandall/docker/jenkins:/var/jenkins_home \
  --name jenkins \
  jenkins/jenkins:lts