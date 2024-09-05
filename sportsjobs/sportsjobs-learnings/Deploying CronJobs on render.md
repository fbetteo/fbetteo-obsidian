
Para evitar correr manualmente todos los scripts que obtienen job posts mediante API y luego subirlos a airtable , indexar y postear en reddit (todos scripts distintos que corria a mano diariamente) arme un cronjob en render.
Cada script es rapido y tardaba unos min por dia pero tenia que encargarme.

Muy sencillo en render con los cron-job.
Permite correr directo desde un repo de github. Necesitas el requirements.txt para que cree el servidor con lo necesario (cual docker).
Permite agregar env variables y secret files lo cual me vino bien porque tenia Keys tanto en Env como en un json file.

Para correr arme un script en bash que llama a todos los scripts. Chatgpt para armarlo bien y por si falla algo que siga andando y etc.
Luego seteas el schedule y voila.

Nota: Despues para relugroup encontre que se puede hacer lo mismo (o casi igual) con github actions de manera gratuita. Es levemente mas complejo porque hay que armar el yml pero me funciono para lo que es scrapear google flights.
