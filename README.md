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


        `expo install firebase react-native-paper`

3. Opret nu en authentication-database i Firebase;
- Følg dette link: https://firebase.google.com/
- Tryk på "Get Started"
- Tryk på "Add Project"
- Giv projektet et vilkårligt navn og tryk "continue"
- Fjern Analytics og tryk "create project"
- Registrer nu en  web Aapplikation. Tryk på ikonet </> - Se billeder herunder;
![img_1](https://user-images.githubusercontent.com/55731954/128145049-ee6b0029-1372-45d6-b6ec-0fefaf49e18b.png)
  
- Giv applikationen et vilkårligt navn og tryk "Register app".
- Du vil nu få præsenteret en kodeblok. Kopiér den del, som er omkranset af en rød boks på billedet herunder;<br/>Erstat `var` med `const`
![img_2](https://user-images.githubusercontent.com/55731954/128145076-eb2c1a95-643d-434b-8dc8-f43ed9e65e4d.png)
  
- Tryk på "Authentication"
- Tryk på "Get Started"
- Aktiver sign-in ved brug af Email/Password
<br/>
  
  ## App.js 

4. Vend tilbage til dit IDE(Webtstorm, VC, Phpstorm, el.lign.)
- Indsæt den kode, som er kopieret fra firebase i App.js
- Opret en components mappe med en js-fil i. Kald filen `SignUpForm`

## SignUpForm 

HINT: Brug løbende den officielle docs:<br/> https://docs.expo.io/guides/using-firebase/

1. importer `firebase` og `useState`
   - Dokumentatoinen på useState kan findes på følgende link:<br/> https://reactjs.org/docs/hooks-state.html
2. Opret const's til; email, password og isCompleted <br/>Eksempel på syntaksen for én af variablerne: `const [email, setEmail] = useState('')`
3. i `return()` oprettes en overskrift og to inputfelter, som skal tage imod email og password samt en `<Button/>`der står for aktivering af en brugeroprettelse.
4. i `onPress` skal knappen kalde metoden, som håndterer brugeroprettelsen.<br/> `<Button title="<--Angiv titel her-->" onPress{() => <--metode->} />`
5. Opret derfor metode `handleSubmit`, som har til formål at aktivere en brugeroprettelse i firebase. 
      - HINT: https://firebase.google.com/docs/auth/web/password-auth.  
      - HUSK: Input felterne skal i `onChangeText` dynamisk sætte værdierne for email & password
6. Importér `SignUpForm` i App.js og placér komponenten i `return()`
7. Sørg for at din kode afprøver om en firebase app allerede er initialiseret, før selve initialiseringen opstår
   - HINT: https://dev.to/lxnd77/comment/13fbb
8. Start nu appen og opret en testbruger. F.eks.<br/>E-mail: test@mail.dk<br/> password: 1234
- Gå ind Firebase -> Authentication -> Users og se se at din bruger nu er oprettet i dit projekt. 

## LoginForm 

1. Opret en js-fil i components-mappen
2. `LoginForm` er kodemæssigt næsten nøjagtig ens med `SignUpForm`. Kopiér derfor al kode fra `SignUpForm` og placér derfor denne i `LoginForm` 
- HUSK: du skal justere komponentens titel. 
3. Justér teksten på `<Button/>` fra at være "create user" til "login"
4. Dernæst skal knappen aktivere en loginmetode fremfor en sign-in metode.
- HINT: Firebase stiller en prædefineret metode til rådighed ligesom med signIn metoden:<br/> https://firebase.google.com/docs/reference/js/firebase.auth.Auth#signinwithemailandpassword

## ProfileScreen

1. Opret en js.fil i components mappen ved navn "ProfileScreen".
2. Opret en `<Text/>`, der udskriver e-mailen på den bruger, som er logget ind. 
-  HINT: Led efter en metode i dokumentationen, som henter oplysninger på en aktiv bruger. 
3. Opret en log ud knap.<br/>HINT: Firebase stiller en log-ud metode til rådighed. Find metoden i dokumentationen.

## App.js

1. Vend tilbage til App.js og opret en metode, som monitorerer om en brugers tilstand ændres fra inaktiv til aktiv
- HINT: Find hjælp på dette link: https://johnwcassidy.medium.com/firebase-authentication-hooks-and-context-d0e47395f402
- Dokumentionen på useEffect: https://reactjs.org/docs/hooks-effect.html
2. Afslutningsvis, skal `return()` i app.js, på baggrund af brugerens tilstand, fremvise enten LoginForm & SignUpForm eller ProfileScreen<br/>Dette kan gøres på flere måde, herunder, if/else eller Tenary
3. Når du starter appen skal du indledningsvist få fremvist en side, der indeholder både felter til login- og oprettelse af en bruger. Ved at oprette en bruger eller login, skal appen automatisk føre dig ind til `Profilescreen`, hvor mailadressen på den aktive bruger fremvises. Ved tryk på log-ud-knappen, skal appen føre dig tilbage til udgangspunktet(sign-up & login).


## Videre arbejder
- Forsøg at adskille login- og sign-up siden
   - Her kan Tabnavigator, Stcknavigator eller Drawernavigator benyttes.
- Opret fejlhåndtering ved eksempelvis dét tilfælde at brugeren indskriver en email uden brug af "@".

## Bilag

### Bilag A - Package.json - Fra Endelig Løsning <br/>
![img_3](https://user-images.githubusercontent.com/55731954/128145201-aa0d6023-5ac9-4ff4-a6ac-43699fdad65e.png)





