
https://www.youtube.com/watch?v=OYpsNaBU_O8&list=PLhH3UpV2flrwfJ2aSwn8MkCKz9VzO-1P4&index=2



Tenes una tabla con users (email, pass)

Para crear usuarios, primero chquea que no exista (query a la base, nada loco)

Para crear el usuario, pasa a la base el user y el password hasheado (passlib)


Genera func "authenticate_user" que requiere, user y pass. Verifica que exista el user y el hashed password


Genera funcion create_token que usa la libreria pyjwt para generar un token (bearer)

Genera ademas un post endpoint que genera token usando Auth0 (un request form parece) que le pasa el user y pass