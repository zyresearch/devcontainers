FROM alpine:latest AS installer
WORKDIR /install-tl-unx
RUN apk add --no-cache \
    curl \
    make \
    perl \
    tar \
    wget \
    xz \
    fontconfig
COPY ./texlive.profile ./
ARG REPO=https://mirrors.tuna.tsinghua.edu.cn/CTAN/systems/texlive/tlnet
RUN wget ${REPO}/install-tl-unx.tar.gz \
    && tar -xzf ./install-tl-unx.tar.gz --strip-components=1 \
    && ./install-tl -repository ${REPO} --profile=texlive.profile \
    && ln -sf /usr/local/texlive/*/bin/* /usr/local/bin/texlive
ENV PATH /usr/local/bin/texlive:$PATH
RUN tlmgr install latexmk chktex latexindent

# latexindent perl modules
RUN cd /usr/local/bin && \
    curl -L https://cpanmin.us/ -o cpanm && \
    chmod +x cpanm
RUN cpanm -n File::HomeDir \
    && cpanm -n Params::ValidationCompiler \
    && cpanm -n YAML::Tiny

FROM alpine:latest
RUN apk add --no-cache perl git wget
COPY --from=installer /usr/local/texlive /usr/local/texlive
COPY --from=installer /usr/local/share/perl5/site_perl /usr/local/share/perl5/site_perl

RUN ln -sf /usr/local/texlive/*/bin/* /usr/local/bin/texlive
ENV PATH /usr/local/bin/texlive:$PATH