FROM centos:centos6

ADD init.sh /usr/bin/


RUN echo 'deltarpm=0' >> /etc/yum.conf && \
    sed -i 's/^mirrorlist=/#mirrorlist=/' /etc/yum.repos.d/* && \
    sed -i 's/^#baseurl=/baseurl=/' /etc/yum.repos.d/* && \
    sed -i 's/enabled=1/enabled=0/' /etc/yum/pluginconf.d/fastestmirror.conf;

RUN yum clean all


# update
RUN yum -y update;

# build deps
RUN yum -y install make gcc libcurl-devel zlib-devel nss-devel openssl-devel git openssh-server \
	fedora-packager python-ctypes libidn-devel rpmbuild yum-utils perl-JSON libtool libtool-ltdl-devel \
	perl-Digest-HMAC perl-Digest-SHA perl-Time-HiRes \
	ccache \
	ocaml ocaml-camlp4 ocaml-camlp4-devel ocaml-compiler-libs ocaml-findlib ocaml-findlib-devel ocaml-runtime \
	s3cmd tcpdump tmux screen wget vim yajl-devel sudo tar

# add makerpm account
RUN /usr/sbin/useradd makerpm && \
	usermod -G wheel makerpm && \
	sed -i 's/^# %wheel.*$/%wheel ALL = (ALL) NOPASSWD: ALL/' /etc/sudoers

# allow to upload packages to the indians
ADD rpmmacros /home/makerpm/.rpmmacros
ADD packages  /home/makerpm/packages
ADD skylable-sx.spec /home/makerpm/
ADD skel/ /root/
ADD build.sh /usr/bin/


# fix permissions on makerpm
RUN chown -R makerpm.makerpm /home/makerpm/;

RUN yum-builddep -y /home/makerpm/skylable-sx.spec 

