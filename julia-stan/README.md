# docker-julia-stan-example

Example Dockerfile for using Julia and Stan.jl

## Build

```
$ docker build -t docker-julia-stan-example:latest .
```

## Run

```
$ docker run --rm -it docker-julia-stan-example:latest

julia> using Mamba, Stan

julia> ProjDir = "."

julia> const stancode = "
       data {
         int<lower=0> N;
         int<lower=0,upper=1> y[N];
       }
       parameters {
         real<lower=0,upper=1> theta;
       }
       model {
         theta ~ beta(1,1);
         y ~ bernoulli(theta);
       }
       "

julia> stanmodel = Stanmodel(name="bernoulli", model=stancode)

julia> const data = Dict("N" => 10, "y" => [0, 1, 0, 1, 0, 0, 0, 0, 0, 1])

julia> retcode, simulation = stan(stanmodel, [data], ProjDir, CmdStanDir=CMDSTAN_HOME)

julia> simulation[1:1000, ["lp__", "theta", "accept_stat__"], :]
Object of type "Mamba.Chains"

Iterations = 1:1000
Thinning interval = 1
Chains = 1,2,3,4
Samples per chain = 1000

[-7.64931 0.313232 0.991105; -7.64931 0.313232 0.788151; … ; -7.83025 0.253144 0.83631; -7.6475 0.314928 0.969623]

[-8.11499 0.211513 0.886084; -9.10924 0.138039 0.937064; … ; -8.96857 0.567249 0.934353; -8.2621 0.196164 1.0]

[-7.68091 0.294405 0.978258; -8.55765 0.527659 0.786427; … ; -7.63825 0.331574 0.993931; -7.73128 0.393559 0.976246]

[-7.63893 0.338641 0.999181; -7.63853 0.329698 0.999297; … ; -8.38751 0.184884 0.97325; -8.23749 0.198553 1.0]
```
