# Docker image file that describes an Centos7 image with UCS PowerTool Core and PowerShell installed from Microsoft YUM Repo

FROM centos:7

LABEL maintainer="Cisco UCS PowerTool Team <ucs-powertool@cisco.com>"
LABEL readme.md="https://github.com/sumanthbr/ciscoucspowertoolcore/blob/master/README.md"
LABEL description="This Dockerfile will install the UCS PowerTool Core Suite for the Centos7 version of PowerShell."

# Setup the locale
ENV LANG en_US.UTF-8
ENV LC_ALL $LANG
RUN localedef --charmap=UTF-8 --inputfile=en_US $LANG

# Install dependencies and clean up
RUN yum install -y \       
        curl \
    && yum clean all

# Download and configure Microsoft Repository config file
RUN curl https://packages.microsoft.com/config/rhel/7/prod.repo| tee /etc/yum.repos.d/microsoft.repo

# Install latest powershell from Microsoft YUM Repo
RUN yum install -y \
        powershell

# Install Unzip package
RUN yum install -y \
		unzip

# Install Lib Curl required for the PowerShell
RUN curl ftp://ftp.pbone.net/mirror/ftp5.gwdg.de/pub/opensuse/repositories/home:/Scott_Cantor/CentOS_7/x86_64/libcurl-openssl-7.43.0-1.1.x86_64.rpm -o /tmp/libcurl-openssl-7.43.0-1.1.x86_64.rpm
RUN yum install -y \
		/tmp/libcurl-openssl-7.43.0-1.1.x86_64.rpm
RUN export LD_LIBRARY_PATH=/opt/shibboleth/lib64/:$LD_LIBRARY_PATH

# Copy UCS PowerTool Suite binaries from local system
RUN mkdir -p ~/.local/share/powershell/Modules
ADD https://communities.cisco.com/servlet/JiveServlet/download/74217-2-149644/ucspowertoolcore.zip /tmp
RUN unzip /tmp/ucspowertoolcore.zip -d ~/.local/share/powershell/Modules/
ADD https://github.com/sumanthbr/ciscoucspowertoolcore/blob/master/DockerFiles/CentOS7/runpowershell.sh /tmp
RUN ["chmod", "+x", "/tmp/runpowershell.sh"]

# Use PowerShell as the default shell
# Use array to avoid Docker prepending /bin/sh -c
ENTRYPOINT ["/tmp/runpowershell.sh"]