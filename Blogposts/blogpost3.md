### 08/10/2024 - 22/10/2024

#### Project Summary

##### Task 1 : Update of tracking of monsters and introducing a simple state system

yea boy, its me.

Last time I build up some simple tracking for the cards, but I wanted to make it a bit more dynamic. But i wasn't that happy with it, since it was a bit of a pain of adding more logic to it, and using file names as a sorting form didn't feel like a good solution.

So i removed the card logic from the AR Image Tracker listener into a monster manager.

In here, you now instantiate the monster obj, and update them.

```csharp
[SerializeField] private MonsterInstance[] monsters;
    public void InstantiateMonster(string imgName, Vector3 position, Quaternion rotation)
    {
        MonsterInstance monster = FindMonsterFromImage(imgName);
        if (monster != null)
        {
            //Debug.Log("Instantiating Monster: " + monster.trackedImage.name + " at " + position);
            Debug.Log("prefab scale: " + monster.prefab.transform.localScale);
            GameObject monsterInstance = Instantiate(monster.prefab, position, rotation);
            IParentCardUpdater parentCardUpdater = monsterInstance.GetComponent<IParentCardUpdater>();
            if (parentCardUpdater != null)
            {
                parentCardUpdater.Init(position, rotation, monster.prefab);
            }
            else
            {
                throw new System.Exception("Missing IParentCardUpdater in Monster Prefab: " + monster.prefab.name);
            }
            monster.Instance = parentCardUpdater;
        }
    }

    public void UpdateMonsterPosition(string imgName, Vector3 position, Quaternion rotation)
    {
        MonsterInstance monster = FindMonsterFromImage(imgName);
        if (monster != null)
        {
            Debug.Log("Updating Monster: " + monster.trackedImage.name + " at " + position);
            //monster.Instance.transform.position = position;
            monster.Instance.UpdateParentCard(position, rotation);
        }
    } ... more code after here

    [System.Serializable]
    public class MonsterInstance
    {
        [SerializeField] public GameObject prefab;
        [SerializeField] public Texture2D trackedImage;
        private IParentCardUpdater instance;

        public IParentCardUpdater Instance
        {
            get => instance;
            set => instance = value;
        }
    }
}
```

In here you simple state the image its connected to, the Monster instance has a prefab and a texture2D.

The MonsterInstance class also has a IParentCardUpdater, which is a interface that is used to update the monster on whats happening with its parent card.

```csharp
public interface IParentCardUpdater
{
    void Init(Vector3 position, Quaternion rotation, GameObject parentCard);
    void UpdateParentCard(Vector3 position, Quaternion rotation);
}
```

I also implemented some state logic in the monster manager, which is used to update the monster state.

Right now the monsters can be in 4 states, Inactive, Spawning, Idle and Despawning.

Right now in idle, the monster simply moves in a lerp towards a wanted position.

```csharp
    public void Update()
    {
        monster.transform.position = Vector3.Lerp(monster.transform.position, monster.CalculateWantedMonPosition(), Time.deltaTime * idleMovementSpeed);
        monster.transform.rotation = Quaternion.Lerp(monster.transform.rotation, monster.parentCardRotation, Time.deltaTime * idleRotationSpeed);
        monster.despawnState.Update();
    }
```

But I did want something more advanced using either a bezier curve or give each monster a curve factor, which determines how tight a curve they can do while moving.

here is some weird drawing I did:

![alt text](IdeasPokething.png)

![alt text](<Skærmbillede 2024-10-27 153712.png>)

don't know how much sense it makes for others, but me brainy know what it means :)

##### Task 2 : Setup multiplayer

We want to feature a _'fighting'_ system in the game between 2 different users of the app.

To accommodate this we decided to use the **PUN 2**(photon) library. Photon provides a simple to get started solution to handling user-to-user interactions.
We're going to be using the built in _lobby_ system provided by photon to setup a peer-to-peer connection between 2(_or more_) users.

The photon network is using a _Master server_ to handle interactions between users. On the photon website we acquired a small student server, and providing the _app_id_ of our project to the server, it will automaticaly handle the networking part by using the photon library in Unity.

Idealy we want the players to be able to scan each other in some form and join a lobby with each other without the need of a whole different scene to set up the networking part. For now though this part is for prototyping user-to-user interactions in our project. There haven't been any major considerations in this part as the goal was to get networking to work in our AR project.

Below is a small technical explanation about how we used the Photon library and how it works behind the scenes.

###### Creating room & Joining a room:

```csharp
private void Start()
    {
        PhotonNetwork.ConnectUsingSettings();
    }
```

When the game is started the player will be connected to the photon server. The **PhotonNetwork** keyword can then be used to do other networking actions on the Photon server.

![image](https://github.com/user-attachments/assets/5d3cdb3f-444c-4e28-be1f-f73b65b9f4fd)
When creating a room(_lobby_) the user will be connected to our photon server and a room is created if the checks passes. The _creator_ of the room will be set as a _master_client_ which can be used to give special rights as the _creator_ of a room.

Joining a room the player will be connected to the room with the provided argument on the photon server.

###### Starting game:

```csharp
 if (PhotonNetwork.IsMasterClient)
    {
        startGameBtn.interactable = true;
    }
public void OnStartGameBtn()
    {
        NetworkManager.instance.photonView.RPC("ChangeScene", RpcTarget.All, "Game");
    }
```

If the player is the _master client_ of the rooom they can use the **Start game button**. When the button is pressed a _RPC call_ is sent out.
RPC(_remote procedure callbacks_) calls are how a request is sent to master server. As seen above we're telling the server to Activate the "_ChangeScene_" method and it should affect all connected clients in the 2nd argument.

![image](https://github.com/user-attachments/assets/a23a7775-843a-4ef6-8021-ccfcc8306bd2)
After the request is handled by the server it returns the Callback on the **ChangeScene** function.
As seen in the screenshot it's annotated with `[PunRPC]` this means that this method can be used a Remote Procedure Callback.
This method will then change the scene on all clients connected to the room.

##### Task 3 : Animate SDJ-course-mon

- Merethe :)

##### Next steps

We still have some features we want the project to have before we feel like our MVP is complete. Below is some of the key elements we'll be working on for our (_probably_) last devlog for **project schoolmon**

###### Fighting

To have a complete loop it makes sense for us, that two players having joined the same lobby is able to fight each other with their mons.
The key elements we need to make before this feature is complete, from our understanding is:

- Healthbar to the mons
- Attacking the opposing mon, by answering 1 of 4 questions
  - Simple system to calculate damage done to healthbar
  - '_despawning'_ mon when health < 0

###### Networking in the game

We have done the setup, and can now connect to players to our photon masterserver. However we have yet to actualize using it in the games scene.
Some of the key elements we want to work on for this part is:

- Finding a way to handle whos mon is whos
- Setting up turnbased actions

###### Missing states

- What should happen when a card cant be found anymore, when the camera is looking at the position where the card should be.
- The switch mon state is not gonna work as intended, we will only have 2 mons in game, one for each player, so no need for switching.

###### Animations??

Er der noget logik her der skal laves her Merethe til de forskellige animationer? :D
