### 08/10/2024 - 22/10/2024

#### Project Summary

##### Task 1 : Setup tracking between physical cards & virtual 'mons'


##### Task 2 : Setup multiplayer
We want to feature a _'fighting'_ system in the game between 2 different users of the app.

To accommodate this we decided to use the **PUN 2**(photon) library. Photon provides a simple to get started solution to handling user-to-user interactions.
We're going to be using the built in _lobby_ system provided by photon to setup a peer-to-peer connection between 2(_or more_) users.

The photon network is using a _Master server_ to handle interactions between users. On the photon website we acquired a small student server, and providing the _app_id_ of our project to the server, it will automaticaly handle the networking part by using the photon library in Unity.

Idealy we want the players to able to scan each other in some form and join a lobby with each other without the need of a whole different scene. For now though this part is for prototyping user-to-user interactions in our project. There haven't been any major considerations in this part as the goal was to get networking to work in our AR project.

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
As seen in the screenshot it's annotated with ```[RPC]``` this means that this method can be used a Remote Procedure Callback.
This method will then change the scene on all clients connected to the room.

##### Task 3 : Animate SDJ-course-mon
