FROM public.ecr.aws/lambda/python:3.9 as builder

COPY pyproject.toml poetry.lock ./
RUN pip install --upgrade pip \
    && pip install poetry \
    && poetry config virtualenvs.create false \
    && poetry install
COPY backend/src/handler.py backend/src/handler.py
COPY backend/src/rpf_account/ backend/src/rpf_account/
COPY backend/tests/ backend/tests/

FROM public.ecr.aws/lambda/python:3.9 as runner
COPY --from=builder /var/lang/bin /var/lang/bin
COPY --from=builder /var/lang/lib/python3.9/site-packages /var/lang/lib/python3.9/site-packages
COPY --from=builder /var/task /var/task
ENV PYTHONPATH="$PYTHONPATH:/var/task/backend/src"