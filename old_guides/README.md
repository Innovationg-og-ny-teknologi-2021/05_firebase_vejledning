# Øvelsesvejldning øvelse 4 - Firebase

## Slutprodukt

https://user-images.githubusercontent.com/48329669/128593607-d93036b5-331f-4c92-a17d-27c7b92244b9.mp4



## Integration med Firebase
[comment]: <> (This is a comment, it will not be included)

1.   Start med at oprette et nyt projekt. 
2.   Installér følgende dependencies;
    - firebase
    - react-native-paper
        - Et skærmklip af package.json filen til den endelige løsning er vedlagt i bilag A.
          Din package.json bør være ens med denne efter installeringen. 
        - KOPIER linjen herunder til installering.


        `npx expo install firebase react-native-paper`

3. Opret nu en authentication-database i Firebase;
- Følg dette link: https://firebase.google.com/
- Tryk på "Get Started"
- Tryk på "Add Project"
- Giv projektet et vilkårligt navn og tryk "continue"
- Fjern Analytics og tryk "create project"
- Registrer nu en  web Aapplikation. Tryk på ikonet </> - Se billeder herunder;
![img_1](https://user-images.githubusercontent.com/55731954/128145049-ee6b0029-1372-45d6-b6ec-0fefaf49e18b.png)
  
- Giv applikationen et vilkårligt navn og tryk "Register app".
- Du vil nu få præsenteret en kodeblok. Kopiér den del, som er omkranset af en rød boks på billedet herunder;
!<img width="586" alt="Skærmbillede 2022-09-19 kl  17 08 22" src="https://user-images.githubusercontent.com/111279752/191050825-a7a61a96-9e08-4cba-9aae-22e1b2cba631.png">

  
- Tryk på "Authentication"
- Tryk på "Get Started"
- Aktiver sign-in ved brug af Email/Password
<br/>
  

metro.config.js - KUN Hvis i vil bruge en ældre version af firebase
=======
## metro.config.js
Da firebase blev opdateret her i slut August 2022, er der stadig en masse bugs som skaber problemer. Et af dem er følgende
Denne fejl vil i modtage når i forsøger at compile jeres applikation på jeres mobil.

Læs om løsning hertil her: https://stackoverflow.com/questions/72179070/react-native-bundling-failure-error-message-while-trying-to-resolve-module-i

  ## App.js 

4. Vend tilbage til dit IDE(Webtstorm, VC, Phpstorm, el.lign.)
- Indsæt den kode, som er kopieret fra firebase i App.js under jeres imports
- Husk også at import ``import { getApps, initializeApp } from "firebase/app";``
- Opret en components mappe med en js-fil i. Kald filen `SignUpForm`

## SignUpForm 

<<<<<<< HEAD
HINT: Brug løbende den officielle docs:<br/> https://docs.expo.io/guides/using-firebase/ <br/>
### OBS. Vi bruger web-version 9 da den er det nye understøttet modular format. I er også velkommen til at bruge det gammel version 8 hvis i heller vil det.

1. importer `firebase` således: ``import { getAuth, createUserWithEmailAndPassword } from "firebase/auth";``
   - I skal huske at impotere de moduler fra firebase i bruger. I dette tilfælde er det `getAuth` og `createUserWithEmailAndPassword`
   - !! OBS !!  - Hvis i benytter jer af Version 8 af firebase - skal i importer firebase's compatibility fil. Dette gør i ved at skrive følgende `import firebase from "firebase/compat";`.
2. Importér `useState` fra react.
   - Dokumentatoinen på useState kan findes på følgende link:<br/> https://reactjs.org/docs/hooks-state.html
3. Opret const til; 
   1. email, 
   2. password 
   3. ``const [isCompleted, setCompleted] = useState(false)``
   4. ``const [errorMessage, setErrorMessage] = useState(null) ``
   5. ``const auth = getAuth();``
   
    <br/>Eksempel på syntaksen for én af variablerne: `const [email, setEmail] = useState('')`


3. i `return()` oprettes en overskrift og to inputfelter, som skal tage imod email og password samt en `<Button/>`der står for aktivering af en brugeroprettelse.
    - Huske at i kan skal indramme jeres inputfelter felterne i et `<TextInput />`
=======
HINT: Brug løbende den officielle dokumentation:<br/> https://docs.expo.io/guides/using-firebase/ <br/>
### OBS. Vi bruger web-version 8 da den er det mest kendte og understøttet format. I er velkommen til at prøve at bruge version 9, men vi har ikke selv fået sat os ind i det.

1. importer `firebase` og `useState`
   - !! OBS !!  - Hvis i benytter jer af Version 8 af firebase (som vi anbefaler lige pt.) - skal i importere firebase's compatibility fil. Dette gør i ved at skrive følgende `import firebase from "firebase/compat";` Gør dette i stedet for bare at importere firebase.
   - Dokumentationen på useState kan findes på følgende link:<br/> https://reactjs.org/docs/hooks-state.html
2. Opret const til; 
   1. email, 
   2. password 
   3. isCompleted 
   <br/>Eksempel på syntaksen for én af variablerne: `const [email, setEmail] = useState('')`
3. i `return()` oprettes en overskrift og to inputfelter, som skal tage imod email og password der står for aktivering af en brugeroprettelse.
    - Huske at i skal indramme jeres inputfelter felterne i et `<TextInput />`
    - ```
      <TextInput
                placeholder="email"
                value={email}
                onChangeText={(email) => setEmail(email)}
                style={styles.inputField}
            /> 
      ```
    - Jeres button kalder i på med `{"JERES BUTTON NAVN"()}`
4. Lav nu jeres button udenfor jeres return, men indenfor jeres SignUpForm funktion. Det en god idé at placere den før jeres return da i kalder på denne button i jeres return.
   - i `onPress` skal knappen kalde metoden, som håndterer brugeroprettelsen.<br/> `<Button title="<--Angiv titel her-->" onPress{() => <--metode->} />`
   - ```//Her defineres brugeroprettelsesknappen, som aktiverer handleSubmit igennem onPress
     const renderButton = () => {
     return <Button onPress={() => handleSubmit()} title="Create user" />;
     }; 
5. Opret derfor metode `handleSubmit`, som har til formål at aktivere en brugeroprettelse i firebase. 
      - HINT: https://firebase.google.com/docs/auth/web/password-auth.  
      - HUSK: Input felterne skal i `onChangeText` dynamisk sætte værdierne for email & password
```
    /*
   * Metoden herunder håndterer oprettelse af brugere ved at anvende den prædefinerede metode, som stilles til rådighed af firebase
   * createUserWithEmailAndPassword tager en mail og et password med som argumenter og foretager et asynkront kald, der eksekverer en brugeroprettelse i firebase https://firebase.google.com/docs/auth/web/password-auth#create_a_password-based_account
   * Opstår der fejl under forsøget på oprettelse, vil der i catch blive fremsat en fejlbesked, som, ved brug af
   * setErrorMessage, angiver værdien for state-variablen, errormessage
   */
      const handleSubmit = async() => {
        await createUserWithEmailAndPassword(auth, email, password)
        .then((userCredential) => {
          // Signed in 
          const user = userCredential.user;
          // ...
        })
        .catch((error) => {
          const errorCode = error.code;
          const errorMessage = error.message;
          setErrorMessage(errorMessage)
          // ..
        });
      }
```


6. Importér `SignUpForm` i jeres App.js og placér komponenten i `return()`
7. Sørg for at din kode afprøver om en firebase app allerede er initialiseret, før selve initialiseringen opstår
   - HINT: https://dev.to/lxnd77/comment/13fbb
   - Hint: Billag B
   - Husk at importer getApps fra firebase/app
8. Start nu appen og opret en testbruger. F.eks.<br/>E-mail: test@mail.dk<br/> password: 1234
- Gå ind Firebase -> Authentication -> Users og se se at din bruger nu er oprettet i dit projekt. 

## LoginForm 

1. Opret en js-fil i components-mappen
2. `LoginForm` er kodemæssigt næsten nøjagtig ens med `SignUpForm`. Kopiér derfor al kode fra `SignUpForm` og placér derfor denne i `LoginForm` 
- HUSK: du skal justere komponentens titel og ændre de firebase componenter i importerer. 
3. Justér teksten på `<Button/>` fra at være "create user" til "login"
4. Dernæst skal knappen aktivere en loginmetode fremfor en sign-in metode.
- HINT: Firebase stiller en prædefineret metode til rådighed ligesom med signIn metoden:<br/> https://firebase.google.com/docs/auth/web/password-auth

## ProfileScreen

1. Opret en js.fil i components mappen ved navn "ProfileScreen".
2. Opret en `<Text/>`, der udskriver e-mailen på den bruger, som er logget ind. 
-  HINT: Led efter en metode i dokumentationen, som henter oplysninger på en aktiv bruger. 
-  HINT: 
```
    const auth = getAuth();
    const user = auth.currentUser
    
    //handleLogout håndterer log ud af en aktiv bruger.
    //Metoden er en prædefineret metode, som firebase stiller tilrådighed  https://firebase.google.com/docs/auth/web/password-auth#next_steps
    //Metoden er et asynkrontkald.
    
    const handleLogOut = async () => {
        await signOut(auth).then(() => {
            // Sign-out successful.
          }).catch((error) => {
            // An error happened.
          });
    };
```
3. Opret en log ud knap.<br/>HINT: Firebase stiller en log-ud metode til rådighed. Find metoden i dokumentationen.

## App.js

1. Vend tilbage til App.js og opret en metode, som monitorerer om en brugers tilstand ændres fra inaktiv til aktiv
- HINT: Find hjælp på dette link: https://johnwcassidy.medium.com/firebase-authentication-hooks-and-context-d0e47395f402
- Dokumentionen på useEffect: https://reactjs.org/docs/hooks-effect.html
2. Afslutningsvis, skal `return()` i app.js, på baggrund af brugerens tilstand, fremvise enten LoginForm & SignUpForm eller ProfileScreen<br/>Dette kan gøres på flere måde, herunder, if/else eller Tenary
3. Når du starter appen skal du indledningsvist få fremvist en side, der indeholder både felter til login- og oprettelse af en bruger. Ved at oprette en bruger eller login, skal appen automatisk føre dig ind til `Profilescreen`, hvor mailadressen på den aktive bruger fremvises. Ved tryk på log-ud-knappen, skal appen føre dig tilbage til udgangspunktet(sign-up & login).


## Videre arbejde
- Forsøg at adskille login- og sign-up siden
   - Her kan Tabnavigator, Stcknavigator eller Drawernavigator benyttes.
- Opret fejlhåndtering ved eksempelvis dét tilfælde at brugeren indskriver en email uden brug af "@".

## Bilag

### Bilag A - Package.json - Fra Endelig Løsning - Jeres versions er ikke det samme <br/>
<img width="344" alt="Skærmbillede 2022-09-19 kl  17 03 10" src="https://user-images.githubusercontent.com/111279752/191049330-5fa21a5f-6f5f-4fc6-a887-7699851eeab8.png">

### Bilag B - 
```
  if (getApps().length < 1) {
    initializeApp(firebaseConfig);
    console.log("Firebase On!");
    // Initialize other firebase products here
  } else {
    console.log("Firebase not on!");
  }

```
