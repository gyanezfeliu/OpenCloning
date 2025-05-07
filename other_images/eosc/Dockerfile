ARG FRONTEND_TAG="prod"
ARG BACKEND_TAG="prod"

FROM manulera/opencloningfrontend:${FRONTEND_TAG} AS frontend

FROM manulera/opencloningbackend:${BACKEND_TAG} AS backend
WORKDIR /home/backend
COPY --from=frontend /build ./frontend
COPY ./docker_entrypoint.sh ./

# To have access to envsubst
USER root
RUN apt-get update && apt-get install -y gettext-base

# Allow any user to overwrite frontend/config.json
RUN chmod 666 ./frontend/config.json
RUN mkdir -p /home/backend/pydna
RUN chmod -R 777 /home/backend/pydna


USER backend

ENV SERVE_FRONTEND=1
ENV ROOT_PATH=""
ENV BACKEND_URL="/"
ENV SHOW_APP_BAR=true
ENV NO_EXTERNAL_REQUESTS=false

ENV pydna_config_dir=/home/backend/pydna
ENV pydna_data_dir=/home/backend/pydna
ENV pydna_log_dir=/home/backend/pydna




CMD sh docker_entrypoint.sh
