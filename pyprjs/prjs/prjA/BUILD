python_sources()

pex_binary(
  name="fp_binary",
  layout="loose",
  execution_mode="venv",
  dependencies=["pyprjs/libs:libs_binary"],
  include_sources=True,
  include_requirements=True,
  venv_site_packages_copies=True
)

docker_image(
  name="test_image",
  instructions=[
        "FROM python:3.11.9-slim as deps",
        "COPY pyprjs.libs/libs_binary.pex /libs_binary.pex",
        "RUN PEX_TOOLS=1 /usr/local/bin/python /libs_binary.pex venv --scope=deps --compile /bin/app",

        "FROM python:3.11.9-slim as srcs",
        "COPY pyprjs.prjs.prjA/fp_binary.pex /fp_binary.pex",
        "RUN PEX_TOOLS=1 /usr/local/bin/python /fp_binary.pex venv --scope=srcs --compile /bin/app",

        "FROM python:3.11.9-slim",
        "COPY --from=deps /bin/app /bin/app",
        "COPY --from=srcs /bin/app /bin/app",
        # "COPY --from=deps /libs_binary.pex /libs_binary.pex",
        # "COPY --from=srcs /fp_binary.pex /fp_binary.pex"
    ]
)
