# LibGDX_Game_Example
# Sprint 1  

xxx edited this page on 29 Sep 2022 
### [Sprint1: Design Eviction Menu](https://github.com/UQdeco2800/2022-ext-studio-1/wiki/Sprint1:-Design-Eviction-Menu)


# [](https://github.com/UQdeco2800/2022-ext-studio-1/wiki)Contents
1. The main menu frame ( this part will be used for displaying several character cards).
3.  The single card for displaying the information of each NPC. (this part will contain each character's information and allow players to select)
4.  The button icon for entering this eviction menu.
5.  The confirming box (When players select one specific NPC whom they regard as the traitor, the confirming box will pop up to ensure)
6. 
## UI design implementation
[1]In order to insert the NPC menu we designed earlier, the[**NpcEvictionMenu.java**](https://github.com/UQdeco2800/2022-ext-studio-1/blob/main/source/core/src/main/com/deco2800/game/screens/NpcEvictionMenu.java) class is created under the .../screens folder.
  
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


In the Npc Eviction MenuDisplay class a table is installed on 8 NPC cards and another table is used to load the background image. An important point is that the order in which the Actors created above are added to the stage is very important. The actor added first is at the bottom, and the actor added last is at the top. So the bgTable Actor for the background image is first added to the stage.
```java
private void addActors() {  
  
    Image backgroundNpcMenu =  
            new Image(  
                    ServiceLocator.getResourceService()  
                            .getAsset("images/eviction_menu/evictionMenu_background.png", Texture.class));  
  
  
    Table menuNpcs = makeNpcCards();  
    /** build new style exit button */  
    Button.ButtonStyle styleExit = new Button.ButtonStyle();  
    styleExit.up = new TextureRegionDrawable(new TextureRegion(  
            new Texture(Gdx.files.internal("images/eviction_menu/exitButton.png"))));  
    //here is for button effect when you pressed on button  
    styleExit.over = new TextureRegionDrawable(new TextureRegion(  
            new Texture(Gdx.files.internal("images/eviction_menu/exitButton_selected.png"))));  
    Button exitBtn = new Button(styleExit);  
    exitBtn.setPosition(EXIT_BUTTON_X_POSITION,EXIT_BUTTON_Y_POSITION);  
    exitBtn.setSize(EXIT_BUTTON_SIZE_WIDTH,EXIT_BUTTON_SIZE_HEIGHT);  
    exitBtn.addListener(  
            new ChangeListener() {  
                @Override  
                public void changed(ChangeEvent changeEvent, Actor actor) {  
                    logger.debug("Exit button clicked");  
                    exitMenu();  
                }  
            });  
    bgTable =new Table();  
    bgTable.setFillParent(true);  
    bgTable.add(backgroundNpcMenu).height(Gdx.graphics.getHeight()-BACKGROUND_HEIGHT_GAP).width(Gdx.graphics.getWidth()-BACKGROUND_WIDTH_GAP);  
  
    rootTable = new Table();  
    rootTable.setFillParent(true);  
    rootTable.add(menuNpcs).center();  
    rootTable.debug();  
  
    **stage.addActor(bgTable);  
    stage.addActor(exitBtn);  
    stage.addActor(rootTable);**  
  
}
```



2.  
Because the traitor is selected during the game after start, the NPC menu icon is placed in the [MainGameExitDisplay.java](https://github.com/UQdeco2800/2022-ext-studio-1/blob/main/source/core/src/main/com/deco2800/game/components/maingame/MainGameExitDisplay.java) class of the maingame folder.

```java
private void addActors() {  
  table = new Table();  
  table.top().right();  
  table.setFillParent(true);  
  /** build new style eviction menu button */  
  Button.ButtonStyle styleEvictionMenu = new Button.ButtonStyle();  
  styleEvictionMenu.over = new TextureRegionDrawable(new TextureRegion(  
          new Texture(Gdx.files.internal("images/eviction_menu/menuIcon_black.png"))));  
  //here is for button effect when you pressed on button  
  styleEvictionMenu.up = new TextureRegionDrawable(new TextureRegion(  
          new Texture(Gdx.files.internal("images/eviction_menu/menuIcon_white.png"))));  
  Button npcMenuBtn = new Button(styleEvictionMenu);  
  TextButton mainMenuBtn = new TextButton("Exit", skin);  
  
  // Triggers an event when the button is pressed.  
  mainMenuBtn.addListener(  
    new ChangeListener() {  
      @Override  
      public void changed(ChangeEvent changeEvent, Actor actor) {  
        logger.debug("Exit button clicked");  
        entity.getEvents().trigger("exit");  
      }  
    });  
  npcMenuBtn.addListener(  
          new ChangeListener() {  
            @Override  
            public void changed(ChangeEvent changeEvent, Actor actor) {  
              logger.debug("Load button clicked");  
              entity.getEvents().trigger("NpcMenu");  
            }  
          });  
  
  table.add(mainMenuBtn).padTop(10f).padRight(10f);  
  table.row();  
  table.add(npcMenuBtn).padTop(15f).width(NPC_MENU_BUTTON_WIDTH).height(NPC_MENU_BUTTON_HEIGHT);  
  table.row();  
  
  stage.addActor(table);  
}
```


### Relevant Files

[**NpcEvictionMenu.java**](https://github.com/UQdeco2800/2022-ext-studio-1/blob/main/source/core/src/main/com/deco2800/game/screens/NpcEvictionMenu.java)

[**NpcEvictionMenuDisplay.java**](https://github.com/UQdeco2800/2022-ext-studio-1/blob/main/source/core/src/main/com/deco2800/game/components/npcEvictionMenu/NpcEvictionMenuDisplay.java)



