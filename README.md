# upvGymSniper

Script en Bash para monitorizar la página de actividades de la UPV y reservar automáticamente plazas de gimnasio cuando se liberan. Incluye dos scripts:

* `upvGymSniper` — se encarga de iniciar sesión, comprobar identificadores y reservar.
* `hoursID` — convierte la selección de día/hora en los identificadores que necesita el bot.

---

## Contenido del repositorio

* `upvGymSniper` : script principal (Bash).
* `hoursID` : script auxiliar para generar `.hours.txt`.

---

## Requisitos

* Bash (posix compatible).
* `curl`.
* Entorno Unix-like (Linux, macOS). Puede funcionar en WSL.
* Acceso válido a la intranet UPV (usuario/contraseña).

---

## Instalación

1. Clona el repo.
2. Da permisos de ejecución:

```bash
chmod +x upvGymSniper hoursID
```

---

## Uso

1. Ejecuta `hoursID` para generar `.hours.txt`:

```bash
./hoursID
```

Sigue las preguntas por consola para seleccionar día, hora y duración. El script escribirá las entradas en `.hours.txt`. Hazlo cada vez que quieras reservar horas en otro día.

2. Ejecuta `upvGymSniper`:

```bash
./upvGymSniper
```

* Si `data.txt` no existe se te pide `usuario` y `contraseña` y se genera `data.txt`.
* Se te pedirá el tiempo de espera (`sleep`) entre comprobaciones.
* El script inicia sesión, monitoriza las horas en `.hours.txt`, y cuando detecta el identificador correspondiente realiza la reserva. Al finalizar cierra sesión y borra `cookies.txt`.

---

## Formato de `.hours.txt`

Cada línea debe contener el identificador en el formato `MUS0NN` generado por `hoursID`. Ejemplo:

```
MUS0123
MUS0124
```

---

## Cómo funciona (resumen técnico)

* `hoursID` convierte día+hora en una serie de ids `MUS0...` según la fórmula del script.
* `upvGymSniper`:

  * inicia sesión enviando `data.txt` a la URL de autenticación.
  * usa cookies para mantener la sesión (`cookies.txt`).
  * para cada id en `.hours.txt` llama a la página de actividades y busca el `p_codgrupo_mat` asociado. Si aparece, ejecuta la petición de matrícula para reservar.
  * cierra sesión con la URL de expiración y elimina `cookies.txt`.

---

## Contacto / notas

Para dudas técnicas sobre el script o ayuda para adaptarlo, comenta en el repositorio.
