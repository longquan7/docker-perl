FROM buildpack-deps:stretch
LABEL maintainer="Peter Martini <PeterCMartini@GMail.com>, Zak B. Elep <zakame@cpan.org>"

COPY *.patch /usr/src/perl/
WORKDIR /usr/src/perl

RUN curl -SL https://cpan.metacpan.org/authors/id/S/SH/SHAY/perl-5.22.4.tar.bz2 -o perl-5.22.4.tar.bz2 \
    && echo '8b3122046d1186598082d0e6da53193b045e85e3505e7d37ee0bdd0bdb539b71 *perl-5.22.4.tar.bz2' | sha256sum -c - \
    && tar --strip-components=1 -xjf perl-5.22.4.tar.bz2 -C /usr/src/perl \
    && rm perl-5.22.4.tar.bz2 \
    && cat *.patch | patch -p1 \
    && ./Configure -Dusethreads -Duse64bitall -Duseshrplib -Dvendorprefix=/usr/local  -des \
    && make -j$(nproc) \
    && TEST_JOBS=$(nproc) make test_harness \
    && make install \
    && cd /usr/src \
    && curl -LO https://raw.githubusercontent.com/miyagawa/cpanminus/master/cpanm \
    && chmod +x cpanm \
    && ./cpanm App::cpanminus \
    && rm -fr ./cpanm /root/.cpanm /usr/src/perl /tmp/*

WORKDIR /root

CMD ["perl5.22.4","-de0"]
