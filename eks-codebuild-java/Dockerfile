FROM alpine

ENV HOME /root
ENV KUBECONFIG $HOME/.kube/kubeconfig

RUN \
  mkdir /root/bin /aws && \
  apk -Uuv add groff less bash python3 py-pip jq curl docker maven openjdk8 npm && \
  pip install --upgrade pip && \
  pip install awscli && \
  apk --purge -v del py-pip && \
  rm /var/cache/apk/* 

ADD https://amazon-eks.s3.us-west-2.amazonaws.com/1.15.10/2020-02-22/bin/linux/amd64/kubectl /usr/local/bin/kubectl

WORKDIR $HOME

COPY entrypoint.sh /usr/local/bin/entrypoint.sh

RUN chmod a+x /usr/local/bin/kubectl /usr/local/bin/entrypoint.sh

ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]