
Supabase has it's own Auth. 
They create the users table for you and manage all the hashing and password things.

they have a library to integrate everything to nextjs easily

The idea is to store the session in cookies

## Signup

With this you get a new user created. Really simple.

`const supabase = createClientComponentClient() //https://supabase.com/docs/guides/auth/auth-helpers/nextjs?language=ts`
`const { error } = await supabase.auth.signUp({`
            `email,`
            `password,`
            `options: {`
                `emailRedirectTo: 'http://localhost:3000/auth/callback', //esto funciona pero no se bien como`
            `},`
        `});`

## Signin

Signing in is as easy.
`const supabase = createClientComponentClient() 
`const { error } = await supabase.auth.signInWithPassword({`
            `email,`
            `password,`
        `});`


## Session

When logged in you get a JWT stored in the cookies (not sure now if I had to do something else apart from the previous steps).
You can retrieve that to validate if there is a user logged in and the permissions it has.

The JWT is the token that encrypts the user and permissions



For example:
   `useEffect(() => {`
        `supabase.auth.getSession()`
            `.then((session) => {`
                `const jwt_token = session?.data?.session?.access_token;`
                `console.log(jwt_token + "pepepep");`
                `if (!jwt_token) {`
                    `console.error("JWT token is not available.");`
                    `return;`
                `}`
                `setJwtToken(jwt_token);`
            `})`
            `.catch(error => console.error('Error getting the token', error));`
        `// Assuming you have a function getJwtToken that synchronously retrieves the JWT token`
    `}, []);`

Then, what I'm doing now is to validate there is a token and if there is, you can call an API point or do whatever you want. Like:

`axios.get<Assistant[]>(process.env.NEXT_PUBLIC_API_URL + '/assistants-protected', {`
            `headers: { "Authorization": Bearer ${jwtToken} }`
            `// Include additional data as needed`
        `})`
            `// .then(response => console.log(response.data))`
            `.then((response) => {`
                `console.log(response.data);`
                `setAssistants(response.data);`
            `})`
            `// .then(console.log(assistants)`
            `.catch(error => console.error('Error fetching channels', error));`


Since I'm using FastAPI in the backend I need to take care manually of the JWT (Would be more straightforward if I used supabase sdk but I need more flexibility).

So, I have a Depend fucntion that validates there is a JWT sent as bearer, decode it and then extract the user id from it.
Then the api call to the db filter results by userid to return only the results of the corresponding user logged in.
Also, Supabase has RLS (row level security) that should enforce this same behavior)