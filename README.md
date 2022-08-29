# LibGDX_Game_Example
# Sprint 1  

xxx edited this page on 29 Sep 2022 
### [Sprint1: Design Eviction Menu](https://github.com/UQdeco2800/2022-ext-studio-1/wiki/Sprint1:-Design-Eviction-Menu)


# [](https://github.com/UQdeco2800/2022-ext-studio-1/wiki)Contents


1.  The main menu frame ( this part will be used for displaying several character cards).
3.  The single card for displaying the information of each NPC. (this part will contain each character's information and allow players to select)
4.  The button icon for entering this eviction menu.
5.  The confirming box (When players select one specific NPC whom they regard as the traitor, the confirming box will pop up to ensure)
## UI design implementation
In order to insert the NPC menu we designed earlier, the[**NpcEvictionMenu.java**](https://github.com/UQdeco2800/2022-ext-studio-1/blob/main/source/core/src/main/com/deco2800/game/screens/NpcEvictionMenu.java) class is created under the .../screens folder.
  
In this class we call addComponent to register the[**NpcEvictionMenuDisplay.java**](https://github.com/UQdeco2800/2022-ext-studio-1/blob/main/source/core/src/main/com/deco2800/game/components/npcEvictionMenu/NpcEvictionMenuDisplay.java) class. The loadAssets function is used to update the background image.

```java
private static final String[] npcEvictionMenuTextures = {"images/eviction_menu/evictionMenu_background.png"};  

public NpcEvictionMenu(GdxGame game) {  
    this.game = game;  
  
    logger.debug("Initialising npc menu screen services");  
    ServiceLocator.registerInputService(new InputService());  
    ServiceLocator.registerResourceService(new ResourceService());  
    ServiceLocator.registerEntityService(new EntityService());  
    ServiceLocator.registerRenderService(new RenderService());  
    renderer = RenderFactory.createRenderer();  
  
    loadAssets();  
    createUI();  
}  
  
*/
private void createUI() {  
    logger.debug("Creating ui");  
    Stage stage = ServiceLocator.getRenderService().getStage();  
    Entity ui = new Entity();  
    ui.addComponent(new NpcEvictionMenuDisplay(game)).addComponent(new InputDecorator(stage, 10));  
    ServiceLocator.getEntityService().register(ui);  
}  
  
*/
private void loadAssets() {  
    logger.debug("Loading assets");  
    ResourceService resourceService = ServiceLocator.getResourceService();  
    resourceService.loadTextures(npcEvictionMenuTextures);  
    ServiceLocator.getResourceService().loadAll();  
}
```

### Relevant Files

[**NpcEvictionMenu.java**](https://github.com/UQdeco2800/2022-ext-studio-1/blob/main/source/core/src/main/com/deco2800/game/screens/NpcEvictionMenu.java)

[**NpcEvictionMenuDisplay.java**](https://github.com/UQdeco2800/2022-ext-studio-1/blob/main/source/core/src/main/com/deco2800/game/components/npcEvictionMenu/NpcEvictionMenuDisplay.java)



## UML Diagrams

### [](https://github.com/UQdeco2800)Class Diagrams

**PlayerActions**

![PlayerActions](https://github.com/UQdeco2800/2021-ext-studio-2/raw/team-4-main-player-character/assets/wiki/mpc/class_PlayerActions.png)

**PlayerAnimationController**

Implementation in [**KeyboardPlayerInputComponent.java**](https://github.com/UQdeco2800/2021-ext-studio-2/blob/main/source/core/src/main/com/deco2800/game/components/player/KeyboardPlayerInputComponent.java):


  @Override
  public boolean keyDown(int keycode) {
    switch (keycode) {

      case Keys.S:
      case Keys.DOWN:
        walkDirection.add(Vector2Utils.DOWN);
        triggerWalkEvent();
        return true;
      case Keys.D:
      case Keys.RIGHT:
        walkDirection.add(Vector2Utils.RIGHT);
        triggerWalkEvent();
        entity.getEvents().trigger(("walkRight"));
        return true;
      case Keys.SPACE:
        entity.getEvents().trigger("attack");
        return true;
      case Keys.W:
      case Keys.UP:
        entity.getEvents().trigger("jump");
        return true;
      default:
        return false;
    }
  }

  /**
   * Triggers player events on specific keycodes.
   *
   * @return whether the input was processed
   * @see InputProcessor#keyUp(int)
   */
  @Override
  public boolean keyUp(int keycode) {
    switch (keycode) {
      case Keys.S:
      case Keys.DOWN:
        walkDirection.sub(Vector2Utils.DOWN);
        triggerWalkEvent();
        return true;
      case Keys.D:
      case Keys.RIGHT:
        walkDirection.sub(Vector2Utils.RIGHT);
        entity.getEvents().trigger(("stopWalkRight"));
        triggerWalkEvent();
        return true;
      case Keys.W:
      case Keys.UP:
        entity.getEvents().trigger("stopJump");
      default:
        return false;
    }
  }

### [](https://github.com/UQdeco2800/2021-ext-studio-2/wiki/Sprint-1-Main-Character-Movement-and-Animation#implementation-of-jump)Implementation of Jump
