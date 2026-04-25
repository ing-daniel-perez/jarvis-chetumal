# Jarvis-Chetumal

Asistente virtual desplegado en clúster Docker Swarm de 2 nodos.

## Nodos
| Nodo | IP | Rol |
|------|----|-----|
| Laptop Lenovo | `10.42.0.1` | Maestro / Gateway |
| PC Ryzen | `10.42.0.154` | Worker / Cálculo |

## Estructura
```
stacks/
  core/     # Infraestructura base (redes, Traefik)
  jarvis/   # Servicios del asistente
configs/    # Configuraciones de servicios
data/       # Volúmenes persistentes (no en git)
secrets/    # Secretos Docker (no en git)
```

## Despliegue
```bash
docker stack deploy -c stacks/core/network.yml core
docker stack deploy -c stacks/jarvis/jarvis.yml jarvis
```
