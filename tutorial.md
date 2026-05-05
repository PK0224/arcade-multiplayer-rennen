# Multiplayer-Rennen mit Funk

## Schritt 1: Spieler erstellen

Erstelle deinen Spieler.

```blocks
let mySprite = sprites.create(assets.image`p2`, SpriteKind.Player)
mySprite.setPosition(20, 30)
```

---

## Schritt 2: Gegner hinzufügen

Erstelle einen Gegner.

```blocks
let myEnemy = sprites.create(assets.image`p3`, SpriteKind.Enemy)
myEnemy.setPosition(20, 60)
```

---

## Schritt 3: Ziel erstellen

Füge ein Ziel hinzu.

```blocks
let finish = sprites.create(assets.image`finish`, SpriteKind.Finish)
finish.setPosition(150, 50)
```

---

## Schritt 4: Funkgruppe wählen

Beide Geräte müssen dieselbe Funkgruppe einstellen.

```blocks
let funcGroup = game.askForNumber("Wählt eure Funkgruppe!", 2)
radio.setGroup(funcGroup)
```

---

## Schritt 5: Spielstart-Variable

Speichere, ob das Spiel gestartet ist.

```blocks
let Spielstart = false
```

---

## Schritt 6: Startsignal senden

Sende ein Startsignal.

```blocks
radio.sendMessage(RadioMessage.Ready)
```

---

## Schritt 7: Startsignal empfangen

Reagiere auf das Startsignal.

```blocks
radio.onReceivedMessage(RadioMessage.Ready, function () {
    Spielstart = true
    radio.sendMessage(RadioMessage.Ready2)
})
```

---

## Schritt 8: Bewegung programmieren

Bewege den Spieler, wenn A gedrückt wird.

```blocks
mp.onButtonEvent(mp.MultiplayerButton.A, ControllerButtonEvent.Pressed, function (player2) {
    if (Spielstart) {
        mySprite.x += 1
        radio.sendNumber(mySprite.x)
    }
})
```

---

## Schritt 9: Gegner bewegen

Empfange die Position des Gegners.

```blocks
radio.onReceivedNumber(function (receivedNumber) {
    myEnemy.x = receivedNumber
})
```

---

## Schritt 10: Gewinner prüfen

Überprüfe, ob jemand das Ziel erreicht.

```blocks
game.onUpdate(function () {
    if (Spielstart) {
        if (mySprite.overlapsWith(finish)) {
            game.over(true)
        } else if (myEnemy.overlapsWith(finish)) {
            game.over(false)
        }
    }
})
```

---

## Bonus-Aufgaben

* Erhöhe die Geschwindigkeit
* Baue Hindernisse ein
* Füge Soundeffekte hinzu
* Gestalte eigene Figuren

---

## Hinweis

Damit das Spiel funktioniert, musst du noch die Radio-Nachrichten definieren:

```blocks
enum RadioMessage {
    Ready = 1,
    Ready2 = 2
}
```
