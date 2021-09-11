# ebiten-manager
A very simple scene manager for ebiten

## Usage
```go
// scenes.MenuScene
var MenuScene *manager.Scene = &manager.Scene{
	Init: func(setReady func()) {
    // this has to be called to guarantee that we don't call anything before Init
		setReady()
	},
	Update: func(setReady func()) error {
		if inpututil.IsKeyJustPressed(ebiten.KeyEnter) {
			Context.Manager.SwitchTo("game")
		}
    // this has to be called to guarantee that we don't call Update before Draw
		setReady()
		return nil
	},
	Draw: func(screen *ebiten.Image) {
		ebitenutil.DebugPrint(screen, "Press ENTER to move to the game")
	},
}

// main.go
import manager "github.com/tducasse/ebiten-manager"

var m *manager.Manager
m = manager.MakeManager(map[string]*manager.Scene{
  "menu": scenes.MenuScene,
  "game": scenes.GameScene,
},
// the first scene 
"menu")

func (game *Game) Update() error {
	return m.Update()
}

func (game *Game) Draw(screen *ebiten.Image) {
	m.Draw(screen)
}

func (game *Game) Layout(w, h int) (int, int) {
	return screenWidth, screenHeight
}

type Game struct{}
var g *Game
g = &Game{}
ebiten.RunGame(g)
```