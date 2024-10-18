# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.245.2/containers/ubuntu/.devcontainer/base.Dockerfile
FROM mcr.microsoft.com/vscode/devcontainers/base:0-ubuntu-22.04

# Run the things we need
RUN apt update && apt install bash-completion && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Select desired version for each tool
ENV ARGOCD_VERSION=v2.10.9
RUN curl -sSL -o /usr/local/bin/argocd "https://github.com/argoproj/argo-cd/releases/download/${ARGOCD_VERSION}/argocd-linux-amd64" \
    && chmod +x /usr/local/bin/argocd \
    && echo 'source <(argocd completion bash)' >> /usr/share/bash-completion/completions/argocd

ENV ARGO_WORKFLOWS_VERSION=v3.5.6
RUN curl -Lo /tmp/argo-linux-amd64.gz "https://github.com/argoproj/argo-workflows/releases/download/${ARGO_WORKFLOWS_VERSION}/argo-linux-amd64.gz" \
    && gunzip /tmp/argo-linux-amd64.gz \
    && mv /tmp/argo-linux-amd64 /usr/local/bin/argo \
    && chmod +x /usr/local/bin/argo \
    && echo 'source <(argo completion bash)' >> /usr/share/bash-completion/completions/argo

ENV ARGO_ROLLOUTS_VERSION=v1.6.6
RUN curl -sSL -o /usr/local/bin/kubectl-argo-rollouts "https://github.com/argoproj/argo-rollouts/releases/download/${ARGO_ROLLOUTS_VERSION}/kubectl-argo-rollouts-linux-amd64" \
    && chmod +x /usr/local/bin/kubectl-argo-rollouts \
    && echo 'source <(kubectl-argo-rollouts completion bash)' >> /usr/share/bash-completion/completions/kubectl-argo-rollouts

ENV AKUITY_VERSION=v0.12.0
RUN curl -sSL -o /usr/local/bin/akuity "https://dl.akuity.io/akuity-cli/${AKUITY_VERSION}/linux/amd64/akuity" \
    && chmod +x /usr/local/bin/akuity \
    && echo 'source <(akuity completion bash)' >> /usr/share/bash-completion/completions/akuity

ENV KIND_VERSION=v0.22.0
RUN curl -Lo /usr/local/bin/kind https://kind.sigs.k8s.io/dl/${KIND_VERSION}/kind-linux-amd64 \
    && chmod +x /usr/local/bin/kind \
    && echo 'source <(kind completion bash)' >> /usr/share/bash-completion/completions/kind

ENV K3D_VERSION=v5.6.3
RUN wget -q -O - https://raw.githubusercontent.com/k3d-io/k3d/${K3D_VERSION}/install.sh | bash

# This should be the same K8s version that kind creates.
ENV KUBECTL_VERSION=v1.29.3
RUN curl -Lo /usr/local/bin/kubectl "https://dl.k8s.io/release/${KUBECTL_VERSION}/bin/linux/amd64/kubectl" \
    && chmod +x /usr/local/bin/kubectl

ENV KUBECTX_VERSION=v0.9.5
RUN curl -Lo /tmp/kubectx.tar.gz "https://github.com/ahmetb/kubectx/releases/download/${KUBECTX_VERSION}/kubectx_${KUBECTX_VERSION}_linux_x86_64.tar.gz" \
    && tar -xvf /tmp/kubectx.tar.gz -C /tmp \
    && mv /tmp/kubectx /usr/local/bin/kubectx \
    && curl -Lo /tmp/kubens.tar.gz "https://github.com/ahmetb/kubectx/releases/download/${KUBECTX_VERSION}/kubens_${KUBECTX_VERSION}_linux_x86_64.tar.gz" \
    && tar -xvf /tmp/kubens.tar.gz -C /tmp \
    && mv /tmp/kubens /usr/local/bin/kubens

# https://github.com/kubernetes-sigs/kustomize/releases
ENV KUSTOMIZE_VERSION=v5.4.1
RUN curl -Lo /tmp/kustomize.tar.gz https://github.com/kubernetes-sigs/kustomize/releases/download/kustomize%2F${KUSTOMIZE_VERSION}/kustomize_${KUSTOMIZE_VERSION}_linux_amd64.tar.gz \
    && tar -xvf /tmp/kustomize.tar.gz -C /tmp \
    && mv /tmp/kustomize /usr/local/bin/kustomize \
    && chmod +x /usr/local/bin/kustomize \
    && echo 'source <(kustomize completion bash)' >> /usr/share/bash-completion/completions/kustomize

ENV HELM_VERSION=3.14.4
RUN curl -LO "https://get.helm.sh/helm-v${HELM_VERSION}-linux-amd64.tar.gz" \
    && tar -zxvf "helm-v${HELM_VERSION}-linux-amd64.tar.gz" \
    && mv linux-amd64/helm /usr/local/bin/ \
    && rm -rf "helm-v${HELM_VERSION}-linux-amd64.tar.gz" linux-amd64 \
    && echo 'source <(helm completion bash)' >> /usr/share/bash-completion/completions/helm

ENV KARGO_VERSION=v0.8.7
RUN curl -Lo /usr/local/bin/kargo "https://github.com/akuity/kargo/releases/download/${KARGO_VERSION}/kargo-linux-amd64" \
    && chmod +x /usr/local/bin/kargo \
    && echo 'source <(kargo completion bash)' >> /usr/share/bash-completion/completions/kargo

# Install yq
ENV YQ_VERSION=v4.43.1
RUN curl -Lo /usr/local/bin/yq "https://github.com/mikefarah/yq/releases/download/${YQ_VERSION}/yq_linux_amd64" \
    && chmod +x /usr/local/bin/yq \
    && echo 'source <(yq shell-completion bash)' >> /usr/share/bash-completion/completions/yq

# Add aliases and such
RUN echo 'source /etc/profile.d/bash_completion.sh' >> /etc/bash.bashrc \
    && echo 'alias k="kubectl"' >> /etc/bash.bashrc \
    && echo 'source <(kubectl completion bash)' >> /etc/bash.bashrc \
    && echo 'complete -F __start_kubectl k' >> /etc/bash.bashrc
