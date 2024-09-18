# 05_firebase-database_vejledning ಠ_ಠ

## Slut produkt
https://user-images.githubusercontent.com/48329669/128408720-8449ae85-8722-4f91-8dc7-0fe58191fb75.mp4

# Integration med Firebase
1. Start med at oprette et nyt projekt som vi plejer. `npx create-expo-app@latest --template blank`
2. Installér følgende dependencies med `npx expo install eller npm install`;
   1. `npx expo install @react-navigation/native @react-navigation/bottom-tabs @react-navigation/stack react-native-vector-icons firebase`
3.  Opret nu en authentication-database i Firebase;
- Følg dette link: https://firebase.google.com/
- Tryk på "Get Started"
- Tryk på "Create a Project"
- Giv projektet et vilkårligt navn og tryk "continue"
- Fjern Analytics og tryk "create project"
- Registrer nu en  web Aapplikation. Tryk på ikonet </> - Se billeder herunder (billederne er ikke helt opdateret men processen er ens);
  ![img_1](https://user-images.githubusercontent.com/55731954/128145049-ee6b0029-1372-45d6-b6ec-0fefaf49e18b.png)

- Giv applikationen et vilkårligt navn og tryk "Register app".

- Gå ind under real time database, og tryk create database. 
- Tryk nu videre og vælg testmode. 
- Gå nu til settings og gå ned i bunden og kopier firebaseConfig-kodestykket nedenfor
<img width="708" alt="Skærmbillede 2022-09-26 kl  11 31 44" src="https://user-images.githubusercontent.com/111279752/192243274-8a1c87ae-3417-4221-8a48-20327db70874.png">

- Indsæt dette i app.js, efter dine imports.

<br> </br>

## Opret app struktur
1. Opret nu en `components` mappe og opret følgende 3 komponenter i den mappe. Brug skabelon 1 i alle tre
   - Add_edit_Car.js 
   - CarDetails.js
   - CarList.js
2. Husk at importere de nødvendige komponenter fra node modules, som `Text` fra react native, præcis som i plejer i alle filer
3. <B> Husk også at komponentnavnet skal være ens med filnavnet </B>

<br> </br>

# App.JS - Opret React navigation ( ˘ ³˘)♥
1. Importer de nødvendige komponenter som vi plejer
   2. Hint: 
   ```javascript
   import { getApps, initializeApp } from "firebase/app"; 
   import {createStackNavigator} from "@react-navigation/stack";
   import {createBottomTabNavigator} from "@react-navigation/bottom-tabs"; 
   ```
2. Efter din const af firebase Configarationen (som du kopieret fra Firebase før), skal Firebase initialiseres:
    ```javascript
        // Vi kontrollerer at der ikke allerede er en initialiseret instans af firebase
        // Så undgår vi fejlen Firebase App named '[DEFAULT]' already exists (app/duplicate-app).
        if (getApps().length < 1) {
            initializeApp(firebaseConfig);
            console.log("Firebase On!");
        // Initialize other firebase products here
        }
    ```

<br> </br>

## Stack og Tab Navigation - App.js
1. Efter overstående kode på App.js, opret en Stack navigatoren med `const Stack = createStackNavigator();`
2. Lav derefter en funktion kaldet StackNavigation, som skal returnere 3 Screens med "name" samt komponentnavnene CarList, CarDetails og Add_edit_car
   1. Det er vigtigt at CarList placeres øverst ;) (Dette er den første skærm brugeren bliver præsenteret med på denne screen)
   2. Se evt. https://reactnavigation.org/docs/stack-navigator/#api-definition
   3. Du kan også se eksempler i tidligere øvelser
3. Oppe ved `const Stack = createStackNavigator()`, Initialiser deraf en Bottom navigatorer med `const Tab = createBottomTabNavigator();`
4. Gå nu til return opret en ``<NavigationContainer></NavigationContainer>``, som skal wrappe din Tab.navigator --> Se https://reactnavigation.org/docs/bottom-tab-navigator/ & https://reactnavigation.org/docs/navigation-container 
5. I Tab.Navigator wrapper oprettes to Tab.Screen med name og component, som henholdvis skal være din StackNavigation og Add_edit_Car
6. Tilføj et ikon til Home Tab.Screen | Eksempel ( husk at Ionicons skal importeres)
``<Tab.Screen name={'Home'} component={StackNavigation} options={{tabBarIcon: () => ( <Ionicons name="home" size={20} />),headerShown:null}}/>``
7. Tilføj nu også et ikon til "Add" Screenen, hvor ikon navnet er ``add`` istedet for home
8. Tilføj nu din `BottomNavigation` i en endelig `return` function. 
9. Nu burde du kunne trykke imellem add og stacknavigatoren "Carlist"

Hint:
```javascript
export default function App() {

 const Stack = createStackNavigator();
 const Tab = createBottomTabNavigator();
 const StackNavigation = () => {
    return(
        <Stack.Navigator>
          <Stack.Screen "din kode her" />
          <Stack.Screen "din kode her" />
          <Stack.Screen "din kode her" />
        </Stack.Navigator>
    )
  }

  const BottomNavigation = () => { 
    return(
      <NavigationContainer>
        <Tab.Navigator>
          <Tab.Screen "din kode her" />
          <Tab.Screen "din kode her" />
        </Tab.Navigator>
      </NavigationContainer>
    )
  }

 return (
   <BottomNavigation/>
  );
}

}
```

<Br> </Br>

# Add_edit_car.js komponent (╯°□°)╯︵ ʞooqǝɔɐɟ
1. Import de nødvendige komponenter som vi plejer
   2. Hint: 
   ```javascript
   import {useEffect, useState} from "react"; import { getDatabase, ref, child, push, update  } from "firebase/database"; 
   ```
2. I parameter parantesen indsæt nu `{navigation,route}` i stedet for `props`, så vi  henter fra react navigations parametre som skal sendes videre fra forskellige Screens
   - Hint: `const Add_edit_Car = ({navigation,route}) => {}`
3. Øverst i din function, lav en db constant med `const db = getDatabase();`
4. Lav derefter en initialState object med tilhørende string attributter: brand,model,year og licensplate 
   ```javascript
   const initialState = {
      brand: '',
      model: '',
      year: '',
      licensePlate: ''
      }
      ```
5. **States** |Lav nedenunder en ny State kaldet `[newCar,setNewCar]`, hvor du i useState bruger `initialState`
    ```javascript
    const [newCar,setNewCar] = useState(initialState);
    ```
6. Lav derunder en const funktion kaldet isEditCar: `const isEditCar = route.name === "Edit car"` eller hvad du nu har kaldt dette screen name i stacknavigatoren i app.js
   1. ( Skal bruges til senere )
7. Opret nu en changeTextInput funktion
   ```javascript 
   const changeTextInput = (name,event) => {
        setNewCar({...newCar, [name]: event});
   }
   ```
8. **Content |** Gå til return og lav et SafeAreaView som parent, og deri et ScrollView
    ```javascript
    return (
            <SafeAreaView style={styles.container}>
                <ScrollView>

                </ScrollView>
            </SafeAreaView>
        );
    ```

   Nu laver vi et über powermove med JS, hvor vi i vores return funktion skal returnere en række felter som vi skal kunne indtaste og ændre værdier i
   ### 1. **Object.keys()**

    - Du har et objekt, som indeholder nogle nøgler (f.eks. `initialState`).
    - Din opgave er at bruge `Object.keys()` for at få en liste over nøglerne fra objektet.
    - **Tip**: `Object.keys()` returnerer et array med alle nøglerne fra objektet, som du kan iterere over.

    ### 2. **map-funktionen**

    - Brug `.map()` for at iterere gennem nøglerne fra `Object.keys()` og generere et inputfelt for hver nøgle.
    - Hvert inputfelt skal have en tilhørende `Text`-komponent, der viser nøglens navn.
    - Husk at give hvert felt en unik nøgle baseret på nøglen eller indekset i arrayet.

    ### 3. **Dynamisk binding til `TextInput`**

    - For hvert felt skal du sørge for, at værdien i `TextInput` svarer til det pågældende element i objektet (f.eks. `newCar`).
    - Brug `onChangeText`-funktionen til at opdatere objektet, når brugeren ændrer indholdet i feltet.
    - **Tip**: Du skal bruge både nøglen og den indtastede værdi (event) for at opdatere det korrekte felt i objektet.

    ### 4. **Konstruktion af komponenterne**

    - Hvert felt skal placeres i en `View`-container med en `Text`-komponent til at vise nøglen og et `TextInput`-felt, hvor brugeren kan indtaste en værdi.
    - **Tænk over**: Hvordan vil du sikre, at både nøgle og værdi er korrekt bundet til inputfeltet? Du skal opdatere en specifik nøgle i objektet, når brugeren skriver i feltet.

### Eksempelvis:

- Hver nøgle skal vises i et tekstfelt, og det tilhørende inputfelt skal være bundet til objektets værdi.
- Tænk over, hvordan du kan bruge `newCar[key]` til at binde værdien til `TextInput` og hvordan du vil opdatere værdien med `onChangeText`.
- <b>Se eventuelt nede i hints  `powermove JS funktion 2`</b>

## HandleSave

1. Lav nu en button som har en save changes funktion kaldet `handlesave()`, og giv en title `Add car`
    - Hint til knappen: 
```javascript
return (
        <SafeAreaView style={styles.container}>
            <ScrollView>
                {
                    Object.keys(initialState).map((key,index) =>{
                        return(
                            <View style={styles.row} key={index}>
                                <Text style={styles.label}>{key}</Text>
                                <TextInput
                                    value={????[key]}
                                    onChangeText={(event) => changeTextInput(key,event)}
                                    style={styles.input}
                                />
                            </View>
                        )
                    })
                }
                {/*Hvis vi er inde på edit car, vis save changes i stedet for add car*/}
                <Button title={ isEditCar ? "Save changes" : "Add car"} onPress={() => handleSave()} />
            </ScrollView>
        </SafeAreaView>
    );
```
2. Kopierer følgende handleSave funktion og indsæt det før din `return`:
```javascript
    const handleSave = async () => {

        const { brand, model, year, licensePlate } = newCar;

        if(brand.length === 0 || model.length === 0 || year.length === 0 || licensePlate.length === 0 ){
            return Alert.alert('Et af felterne er tomme!');
        }

        if(isEditCar){
            const id = route.params.car[0];
            // Define the path to the specific car node you want to update
            const carRef = ref(db, `Cars/${id}`);

            // Define the fields you want to update
            const updatedFields = {
                brand,
                model,
                year,
                licensePlate,
            };
            
            // Use the 'update' function to update the specified fields
            await update(carRef, updatedFields)
                .then(() => {
                Alert.alert("Din info er nu opdateret");
                const car = newCar
                navigation.navigate("Car Details", { car });
                })
                .catch((error) => {
                console.error(`Error: ${error.message}`);
                });

        }else{
        // Define the path to the "Cars" node where you want to push the new data
        const carsRef = ref(db, "/Cars/");
        
        // Data to push
        const newCarData = {
            brand,
            model,
            year,
            licensePlate,
        };
        
        // Push the new data to the "Cars" node
        await push(carsRef, newCarData)
            .then(() => {
            Alert.alert("Saved");
            setNewCar(initialState);
            })
            .catch((error) => {
            console.error(`Error: ${error.message}`);
            });
    }
    };
```
3. <b> Brug lidt tid til at forstå hvad koden gør. Evt. få en LLM til at forklare dig det i bider. </b>

4. Nå du trykker på button i din app burde du nu få en alert med, at du har saved. Gå nu ind i firebase (console.firebase.com) for at se, om din bil er blevet oprettet

<br> </b>

# CarList.js ┬─┬﻿ ノ( ゜-゜ノ)
1. Importer de nødvendige komponenter som vi plejer
2. Indsæt `{navigation}` i parameter parentesen i stedet for props, så vi henter fra react navigations parametre som skal sendes videre fra forskellige Screens
   3. Hint: `function CarList = ({navigation}) => {}`
3. Opret en state for `[cars, setCars]` med `useState()`
4. Lav nu en `useEffect` funktion, hvori der laves en if(!cars) og derefter indsætter denne kode:
    ```javascript
    useEffect(() => {
            const db = getDatabase();
            const carsRef = ref(db, "Cars");
        
            // Use the 'onValue' function to listen for changes in the 'Cars' node
            onValue(carsRef, (snapshot) => {
                const data = snapshot.val();
                if (data) {
                    // If data exists, set it in the 'cars' state
                    setCars(data);
                }
            });
        
            // Clean up the listener when the component unmounts
            return () => {
                // Unsubscribe the listener
                off(carsRef);
            };
        }, []); // The empty dependency array means this effect runs only once

        // Vi viser ingenting hvis der ikke er data
        if (!cars) {
            return <Text>Loading...</Text>;
        }
    ```

    - Husk at useEffect kun skal køres en gang med et tomt array til sidst `[]);`

4. Efter useEffect laves et if statement som kontrollerer, om der ikke er nogle biler og returner
    ```javascript
        // Vi viser ingenting hvis der ikke er data
        if (!cars) {
            return <Text>Loading...</Text>;
        }
    ```
5. Lav nu en funktion kaldet `const handleSelectCar = id => {}`, som søger direkte i vores array af biler og finder bilobjektet, som matcher idet vi har tilsendt og ligger det ned i vores parametre i navigation
    ```javascript
    const handleSelectCar = id => {
            /*Her søger vi direkte i vores array af biler og finder bil objektet som matcher idet vi har tilsendt*/
            const car = Object.entries(cars).find( car => car[0] === id /*id*/)
            navigation.navigate('Car Details', { car });
        };
    ```
6. Over return funktionen her skal vi have to const variabler `carArray` med værdier `Object.values()`, og `carKeys` med `Obejct.keys()`
    ```javascript
    // Flatlist forventer et array. Derfor tager vi alle values fra vores cars objekt, og bruger som array til listen
    const carArray = Object.values(cars);
    const carKeys = Object.keys(cars);
    ```
7. I Return funktionen vil vi lave en `<FlatList/>`, hvori der skal laves følgende tre attributter
   1. `data={carArray}`
   2. `keyExtractor` med en funktion med parametrene item, index som sættes med `carKeys[index]` 
        - ``keyExtractor={(item, index) => carKeys[index]}``)
   3. `renderItem` med følgende return funktion: ``({item,index}) => { return() } ``. 
   4. I return funktionen skal vi have et parent `TouchableOpacity` med en onpress funktion: `handleSelectCar(carKeys[index])`
      1. lav et Text element med `{item.brand}`
      2. Hint ``` <TouchableOpacity style={styles.container} onPress={() => handleSelectCar(carKeys[index])}> ```
    5. Hint:
        ```javascript
        return (
            <FlatList
                data={carArray}
                // Vi bruger carKeys til at finde ID på den aktuelle bil og returnerer dette som key, og giver det med som ID til CarListItem
                keyExtractor={(item, index) => carKeys[index]}
                renderItem={({ item, index }) => {
                    return(
                        <TouchableOpacity style={styles.container} onPress={() => handleSelectCar(carKeys[index])}>
                            <Text>
                                {item.brand} {item.model}
                            </Text>
                        </TouchableOpacity>
                    )
                }}
            />
        );
        ```
9. Nu skulle du gerne kunne trykke på bil-modellen, du har oprettet i tidligere step og gå til car details

<Br> </Br>

# CarDetails (っ˘ڡ˘ς)
1. Impoter de nødvendige komponenter som vi plejer
   - Hint: `import {useEffect, useState} from "react";
      import { getDatabase, ref, remove } from "firebase/database";`
2. Start med at indsætte `{navigation,route}` i stedet for props ved CarDetails præcis som i de andre komponenter

3. Opret en tom state for cars med `useState`
4. Lav nu en `useEffect`, og hent car values og sæt dem med `setCar(route.params.car[1])`. Når vi forlader screenen, skal objektet tømmes: `return () => { setCar({}) }`
   - Hint: 
        ```javascript 
            useEffect(() => {
            setCar(route.params.car[1])
            return () => {
                setCar({})
            };
        }, []);
        ```
5. Opret nu en funktion kaldet `handleEdit`, som henter car objektet fra ``route.params.car``, og sæt det i navigation, så vi navigerer til 'Edit Car' - hint: kig tilbage i navigationsøvelsen
   - hint:   
    ```javascript 
        const handleEdit = () => {
        // Vi navigerer videre til EditCar skærmen og sender bilen videre med
        const car = route.params.car
        navigation.navigate('Edit Car', { car });
        }; 
      ```
6. Lav nu en `confirmDelete` funktion hvor du laver en `Alert.alert`. Vi fremviser vores native alert afhængig af hvilke system brugeren er på:
    ```javascript
    // Vi spørger brugeren om han er sikker
    const confirmDelete = () => {
        /*Er det mobile?*/
        if(Platform.OS ==='ios' || Platform.OS ==='android'){
            Alert.alert('Are you sure?', 'Do you want to delete the car?', [
                { text: 'Cancel', style: 'cancel' },
                // Vi bruger this.handleDelete som eventHandler til onPress
                { text: 'Delete', style: 'destructive', onPress: () => handleDelete() },
            ]);
        }
    };
    ```
7. Lav nu en handleDelete funktion, som først henter id fra route med ``route.params.car[0]`` og lav en try catch, hvor i try'en kaldes næsten den samme firebase funktion som du har hentet "cars" med tidligere, men til sidst tilføj `.remove()` og i ref'en ændres `/Cars/${id}`. I catchen, skal parametren tage `error`som argument og inde i catchen laves en Alert med error.message
   - hint:  
    ```javascript
        const handleDelete = async () => {
                const id = route.params.car[0];
                const db = getDatabase();
                // Define the path to the specific car node you want to remove
                const carRef = ref(db, `Cars/${id}`);
                
                // Use the 'remove' function to delete the car node
            await remove(carRef)
                    .then(() => {
                        navigation.goBack();
                    })
                    .catch((error) => {
                        Alert.alert(error.message);
                    });
            };
    ``` 
8. Nedenfor laves nu et `if` statemtent: hvis der ikke er en car, skal vi returnere et text element med vilkårlig tekst
    ```javascript
    if (!car) {
        return <Text>No data</Text>;
    }
    ```
9. I return funktionen laves to knapper, som kalder på hhv `handleEdit()`, og `confirmDelete()`
10. Prøv at forstå følgende, og indsæt derefter knapper
    ```javascript
        Object.entries(car).map((item,index)=>{
            return(
                <View style={styles.row} key={index}>
                    {/*Vores car keys navn*/}
                    <Text style={styles.label}>{item[0]} </Text>
                    {/*Vores car values navne */}
                    <Text style={styles.value}>{item[1]}</Text>
                </View>
            )
        })
    ```
11. Nu burde du se alle bilens informationer og kunne slette din bil. Og du burde kunne gå til edit/add screen

<br></br>

# Add_edit_Car | step 2 (◐‿◑)﻿
1. Nu vil vi gerne have, at når vi kommer fra CarDetails, så tager den vores bil med og indsætter værdierne i felterne

2. Opret nu en `useEffect` med en funktion
   1. Her i denne funktion skal der først oprettes et if statement, med `isEditCar`
   2. I `if` funktionen, så skal du hente din bils objekt fra paramentrene `route.params.car[1]` og `setNewCar(car)`
   3. Dernæst lav en return med en funktion, som siger `setNewCar(initalState)` for at fjerne data, når vi går væk fra screenen
   4. Husk et tomt array til sidst i `useEffecten`
3. Dernæst i `handleSave` lav et `if else` statement
   1. i if'en brug `isEditCar`, og deri lav en try catch som tager udgangspunkt i følgende https://firebase.google.com/docs/database/web/read-and-write 
   2. dernæst i else'en lig add funktionen 
4. Gå nu ned i titlen på button nederst og sæt et isEditCar ? "Save changes" : "Add car" i titlen
5. done
Hint:
```javascript
const [newCar,setNewCar] = useState(initialState);

    /*Returnere true, hvis vi er på edit car*/
    const isEditCar = route.name === "Edit Car";

    useEffect(() => {
        if(isEditCar){
            const car = route.params.car[1];
            setNewCar(car)
        }
        /*Fjern data, når vi går væk fra screenen*/
        return () => {
            setNewCar(initialState)
        };
    }, []);

    const changeTextInput = (name,event) => {
        setNewCar({...newCar, [name]: event});
    }

    const handleSave = async () => {

        const { brand, model, year, licensePlate } = newCar;

        if(brand.length === 0 || model.length === 0 || year.length === 0 || licensePlate.length === 0 ){
            return Alert.alert('Et af felterne er tomme!');
        }

        if(isEditCar){
            const id = route.params.car[0];
            // Define the path to the specific car node you want to update
            const carRef = ref(db, `Cars/${id}`);

            // Define the fields you want to update
            const updatedFields = {
                brand,
                model,
                year,
                licensePlate,
            };
            
            // Use the 'update' function to update the specified fields
            await update(carRef, updatedFields)
                .then(() => {
                Alert.alert("Din info er nu opdateret");
                const car = newCar
                navigation.navigate("Car Details", { car });
                })
                .catch((error) => {
                console.error(`Error: ${error.message}`);
                });

        }else{
        // Define the path to the "Cars" node where you want to push the new data
        const carsRef = ref(db, "/Cars/");
        
        // Data to push
        const newCarData = {
            brand,
            model,
            year,
            licensePlate,
        };
        
        // Push the new data to the "Cars" node
        await push(carsRef, newCarData)
            .then(() => {
            Alert.alert("Saved");
            setNewCar(initialState);
            })
            .catch((error) => {
            console.error(`Error: ${error.message}`);
            });
    }

```


# Hints 
### Skabelon 1
```javascript
const KomponentNavn = (props) => { 
    return (
        <Text>Det er mit KomponentNavn</Text>
    )
}

export default KomponentNavn; 
```

### powermove JS funktion 2 ( husk turborgklammer når vi er i return )
```javascript
Object.keys(initialState).map((key,index) =>{
   return(
       <View style={styles.row} key={index}>
           <Text style={styles.label}>{key}</Text>
           <TextInput
               value={newCar[key]}
               onChangeText={(event) => changeTextInput(key,event)}
               style={styles.input}
           />
       </View>
   )
})
```

## referencer
https://reactnavigation.org/docs/stack-navigator/
https://reactnavigation.org/docs/bottom-tab-navigator/
UseEffect:https://reactjs.org/docs/hooks-effect.html 
