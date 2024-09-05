
Para tener los jobspost posteados en jobboardsearch necesitaba una API.
Mi idea era pasarle la api de Airtable directamente con un token readme only.
El dueño de jobboardsearch me djio que no podia usar un authorization bearer y etc sino que necesitaba una url nomas.

Mi workaround fue crear una API wrapper de la de airtable. Que yo le pueda pasar el token como parametro en la url y luego esa api le pegara a la de Airtable con el formato que necesita.

Habra una mejor manera?

Requisitos
* Free tier de Render  https://dashboard.render.com/web/srv-cnphuov109ks73eugtng
* Repo para deployar  https://github.com/fbetteo/sportsjobs-jobboardsearch/tree/main

Genere el repo basado en el template. Es simplemente un main con fastapi.
una url GET que le pasas el token.
Luego eso llama a airtable con los parametros que quiero y el token con el formato correcto.

Deployar fue muy facil tambien, basado en el template. 
Elegis deployar desde github, pasas el repo y seguis las configuraciones (puerto y alguna boludes segun la documentacion). Necesitas un requirements.txt (hay alternativas)
Y listo. deploya el repo y queda la api andando.

