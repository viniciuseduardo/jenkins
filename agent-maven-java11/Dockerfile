FROM openshift/jenkins-slave-base-centos7:v3.11

MAINTAINER Vinicius Alves <viniciuseduardo.rj@gmail.com>

ENV DEBIAN_FRONTEND=noninteractive \
    BASH_ENV=/usr/local/bin/scl_enable \
    ENV=/usr/local/bin/scl_enable \
    PROMPT_COMMAND=". /usr/local/bin/scl_enable" \    
    JAVA_HOME=/usr/lib/jvm/java-11-oracle \
    JAVA_VERSION=11.0.2 \
    BUILD=9 \
    SIG=f51449fcd52f4d52b93a989c5c56ed3c \
    MAVEN_VERSION=3.6.0 \
    MAVEN_HOME=/usr/share/maven \
    PATH=$PATH:$MAVEN_HOME:$JAVA_HOME 

#Install Java 11
RUN curl --location --retry 3 \
        --header "Cookie: oraclelicense=accept-securebackup-cookie;" \
        http://download.oracle.com/otn-pub/java/jdk/"${JAVA_VERSION}"+"${BUILD}"/"${SIG}"/jdk-"${JAVA_VERSION}"_linux-x64_bin.tar.gz \
        | tar xz -C /tmp && \
    mkdir -p /usr/lib/jvm && mv /tmp/jdk-${JAVA_VERSION} "${JAVA_HOME}" && \
    yum clean all && \
    rm -rf /tmp/* /var/cache/yum/* /var/tmp/* && \
    update-alternatives --install "/usr/bin/java" "java" "${JAVA_HOME}/bin/java" 1 && \
    update-alternatives --install "/usr/bin/javac" "javac" "${JAVA_HOME}/bin/javac" 1 && \
    update-alternatives --set java "${JAVA_HOME}/bin/java" && \
    update-alternatives --set javac "${JAVA_HOME}/bin/javac"

# Install Maven
RUN curl -fsSL https://archive.apache.org/dist/maven/maven-3/$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz | tar xzf - -C /usr/share \
    && mv /usr/share/apache-maven-$MAVEN_VERSION /usr/share/maven \
    && ln -s /usr/share/maven/bin/mvn /usr/bin/mvn \
    && mkdir -p $HOME/.m2

ADD contrib/bin/* /usr/local/bin/
ADD ./contrib/settings.xml $HOME/.m2/

RUN chown -R 1001:0 $HOME && \
    chmod -R g+rw $HOME

USER 1001
