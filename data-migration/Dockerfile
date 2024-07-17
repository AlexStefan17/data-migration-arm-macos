FROM arm64v8/amazoncorretto

# Set environment variables for JMeter
ENV JMETER_VERSION=5.6.3
ENV JMETER_HOME /opt/apache-jmeter-${JMETER_VERSION}
ENV PATH $JMETER_HOME/bin:$PATH

# Install required packages and download JMeter
RUN apk update && \
    apk add --no-cache wget unzip && \
    wget https://downloads.apache.org//jmeter/binaries/apache-jmeter-${JMETER_VERSION}.tgz && \
    tar -xzf apache-jmeter-${JMETER_VERSION}.tgz -C /opt && \
    rm apache-jmeter-${JMETER_VERSION}.tgz

RUN wget -O ${JMETER_HOME}/lib/ext/jmeter-plugins-manager-1.10.jar https://search.maven.org/remotecontent?filepath=kg/apc/jmeter-plugins-manager/1.10/jmeter-plugins-manager-1.10.jar

# Install Dummy Sampler plugin
RUN wget -O ${JMETER_HOME}/lib/ext/jmeter-plugins-dummy-0.4.jar https://repo1.maven.org/maven2/kg/apc/jmeter-plugins-dummy/0.4/jmeter-plugins-dummy-0.4.jar

# Download CMDRunner
RUN wget -O ${JMETER_HOME}/lib/cmdrunner-2.2.jar https://repo1.maven.org/maven2/kg/apc/cmdrunner/2.2/cmdrunner-2.2.jar

# Add custom jmeter.properties file with CookieManager.save.cookies=true
RUN echo "CookieManager.save.cookies=true" >> ${JMETER_HOME}/bin/jmeter.properties

EXPOSE 1099 50000

WORKDIR $JMETER_HOME

COPY migration.jmx $JMETER_HOME

ENTRYPOINT ["jmeter", "-n", "-t", "migration.jmx"]