FROM julia:0.6.2

ENV CMDSTAN_HOME /usr/local/cmdstan
ENV CMDSTAN_DEPS make g++
ENV CMDSTAN_VER 2.17.1
ENV CMDSTAN_URL https://github.com/stan-dev/cmdstan/releases/download/v${CMDSTAN_VER}/cmdstan-${CMDSTAN_VER}.tar.gz

# Stan.jl requires Mamba.jl optionally, and Mamba.jl requires Cairo.jl
# See:
#   https://github.com/JuliaLang/BinDeps.jl#diagnostics
#   https://github.com/JuliaGraphics/Cairo.jl/blob/master/deps/build.jl
ENV CAIRO_DEPS gettext libglib2.0-0 libcairo2 libpango1.0-0

RUN apt-get update && \
    apt-get install -y ${CMDSTAN_DEPS} ${CAIRO_DEPS} && \
    curl -L -o /tmp/cmdstan-${CMDSTAN_VER}.tar.gz ${CMDSTAN_URL} && \
    tar -xzf /tmp/cmdstan-${CMDSTAN_VER}.tar.gz -C /tmp && \
    (cd /tmp/cmdstan-${CMDSTAN_VER} && make build) && \
    mv /tmp/cmdstan-${CMDSTAN_VER} ${CMDSTAN_HOME} && \
    julia -e 'Pkg.add("Stan"); Pkg.add("Mamba"); using Mamba, Stan'
