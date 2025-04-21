# ♟️ Create & Upload Your Chess Bot to Battle AggressiveBot

qWelcome to **FightScript**! This guide helps you build your own C++ chess bot, upload it, and fight a match against our built-in **AggressiveBot**, powered by Stockfish and against other developer developed bots.

---

## ✅ What You Need

- A working C++ environment (`g++` recommended)
- Basic knowledge of the **UCI (Universal Chess Interface)**
- Your bot should:
  - Read input via `std::cin`
  - Output moves via `std::cout`
  - Handle standard UCI commands

> 🔧 Your final upload **must be a compiled Linux binary (`your_bot`)**, not the source `.cpp` file.

---

## 📄 Sample C++ Bot Code

This example bot picks a basic move depending on the color and follows the UCI protocol strictly:

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <random>
#include <chrono>

std::string get_move(const std::string& fen) {
    bool is_white = fen.find(" w ") != std::string::npos;
    std::vector<std::string> moves;

    if (is_white) {
        moves = {"e2e4", "d2d4", "g1f3", "b1c3", "f1c4", "f1b5"};
    } else {
        moves = {"e7e5", "d7d5", "g8f6", "b8c6", "f8c5", "f8b4"};
    }

    auto seed = std::chrono::system_clock::now().time_since_epoch().count();
    std::mt19937 gen(seed);
    std::uniform_int_distribution<> dis(0, moves.size() - 1);

    return moves[dis(gen)];
}

int main() {
    std::string line;
    std::string current_fen;

    while (std::getline(std::cin, line)) {
        if (line == "uci") {
            std::cout << "id name MyBot\n";
            std::cout << "id author Your Name\n";
            std::cout << "uciok\n";
        } else if (line == "isready") {
            std::cout << "readyok\n";
        } else if (line.rfind("position", 0) == 0) {
            size_t fenIndex = line.find("fen ");
            if (fenIndex != std::string::npos) {
                current_fen = line.substr(fenIndex + 4);
            }
        } else if (line == "go") {
            std::string move = get_move(current_fen);
            std::cout << "bestmove " << move << "\n";
        } else if (line == "quit") {
            break;
        }
    }

    return 0;
}
```

---

## ⚙️ How to Compile (on your local machine)

Use this command to generate an executable Linux-compatible binary:

```bash
g++ -std=c++17 your_bot.cpp -o your_bot
```

Make sure the compiled binary:
- Has **no platform dependencies** (compile on Linux or WSL for best compatibility)
- Is tested locally before upload: `./your_bot` and type in UCI commands
- Has execution permissions: `chmod +x your_bot`

---

## 📤 Uploading Your Bot

Once compiled:

1. Rename it meaningfully, e.g., `my_bot`
2. Upload it using the **Upload Bot** button on the site
3. The server downloads it from Google Drive
4. It’s executed inside a secure environment alongside Stockfish

---

## 🔄 Match Flow

- Your bot is treated like a UCI engine
- Stockfish sends standard `uci`, `isready`, `position`, `go` commands
- Your bot replies with `bestmove XYZ`
- The game ends when one side wins or it's a draw
- You'll get:
  - Game result
  - Move history
  - Winner and reason

---

## ⚠️ Rules & Tips

- Your bot must:
  - Respond to `uci`, `isready`, `position`, `go`, `quit`
  - Output **only UCI responses** (no debug prints to stdout)
- Keep your response time < 1 second
- Avoid memory leaks or background threads
- You can build advanced bots with:
  - Minimax
  - Evaluation functions
  - Libraries like [chesslib](https://github.com/bagaturchess/chesslib)

---

## ✅ Final Upload Checklist

✅ Compiled binary (Linux-compatible)  
✅ Tested with UCI manually  
✅ Named appropriately (no spaces/special characters)  
✅ Set as executable (`chmod +x my_bot`)  
✅ Uploaded via the site interface (https://fightscript.io/competitions/chess)
