python_sources()

kwargs = dict(
    entry_point="prjs.prjA.forecast:main",
    layout="packed",
    execution_mode="venv",
)
pex_binary(
    name="pex-dependencies",
    include_sources=False,
    **kwargs,
)
pex_binary(
    name="pex-sources",
    include_requirements=False,
    **kwargs,
)

docker_image(
    name="img",
    instructions=[
        "FROM python:3.11.9-slim as deps",
        "COPY pyprjs.prjs.prjA/pex-dependencies.pex /pex-dependencies.pex",
        "RUN PEX_TOOLS=1 /usr/local/bin/python /pex-dependencies.pex venv --scope=deps --compile /bin/app",
        "FROM python:3.11.9-slim as srcs",
        "COPY pyprjs.prjs.prjA/pex-sources.pex /pex-sources.pex",
        "RUN PEX_TOOLS=1 /usr/local/bin/python /pex-sources.pex venv --scope=srcs --compile /bin/app",
        "FROM python:3.11.9-slim",
        "COPY --from=deps /bin/app /bin/app",
        "COPY --from=srcs /bin/app /bin/app",
    ],
)
