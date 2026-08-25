# oracle-a1-capacity-retry

Reintenta cada 20 minutos crear una instancia Ampere A1 (Always Free) en Oracle Cloud
mediante el Apply de un stack de Resource Manager ya existente, hasta que haya
capacidad disponible en la región. Avisa por [ntfy.sh](https://ntfy.sh) al conseguirlo.

⚠️ **El disparador es externo**: este repo solo expone `workflow_dispatch`, y quien
marca la cadencia es el Worker de Cloudflare `wine-app/workers/oracle-retry-trigger`.
El cron nativo de GitHub se retiró el 2026-08-18 porque llegaba tarde y desplazado, y
acababa encadenándose con el Worker en lugar de repartirse la hora. Lo que nos frena
no es la capacidad de Oracle sino el **429 `TooManyRequests` del Resource Manager**
(límite por tenant sobre `POST /jobs`): las 24 ejecuciones previas a esa fecha
fallaron todas ahí, sin llegar siquiera a preguntar por capacidad. Por lo mismo la
llamada va con `--no-retry`, para que un intento sea una petición y no ocho.

No contiene credenciales: usa Secrets del repositorio (`OCI_USER`, `OCI_FINGERPRINT`,
`OCI_TENANCY`, `OCI_REGION`, `OCI_KEY_CONTENT`, `OCI_STACK_ID`, `NTFY_TOPIC`).
