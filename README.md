import tkinter as tk
import random
import math
import time

WIDTH, HEIGHT = 1000, 650

BG = "#070b14"
GRID = "#101a2b"
PANEL = "#0b1120"

PLAYER_COLOR = "#38bdf8"
BULLET_COLOR = "#f8fafc"

TEXT = "#f8fafc"
MUTED = "#64748b"

GREEN = "#22c55e"
PURPLE = "#a855f7"
YELLOW = "#facc15"
RED = "#f43f5e"
ORANGE = "#fb923c"
CYAN = "#22d3ee"


# ============================================================
# RARETÉS
# ============================================================

RARITY_COLORS = {
    "Commun": "#94a3b8",
    "Uncommun": "#22c55e",
    "Rare": "#38bdf8",
    "Épique": "#a855f7",
    "Légendaire": "#f59e0b",
    "Mythique": "#f43f5e",
    "Divin": "#facc15"
}


# ============================================================
# COMPÉTENCES
# ============================================================

UPGRADES = [

    # COMMUN

    ("MULTI-TIR", "+1 projectile",
     "Commun", "Projectile", "projectiles", 1),

    ("DÉGÂTS", "+1 dégât",
     "Commun", "Attaque", "damage", 1),

    ("RAPIDITÉ", "+40 vitesse",
     "Commun", "Vitesse", "move_speed", 40),

    ("COEUR", "+25 PV maximum",
     "Commun", "Soin", "max_hp", 25),

    ("PETIT SOIN", "+25 PV",
     "Commun", "Soin", "heal", 25),

    ("PETIT BOUCLIER", "+1 bouclier",
     "Commun", "Défense", "shield", 1),

    ("MUNITIONS RAPIDES", "+100 vitesse de balle",
     "Commun", "Projectile", "bullet_speed", 100),

    ("CALIBRE", "+2 taille des balles",
     "Commun", "Attaque", "bullet_size", 2),

    ("VITALITÉ", "+1 régénération/s",
     "Commun", "Soin", "regen", 1),

    ("VISÉE", "+5% critique",
     "Commun", "Critique", "crit", 0.05),


    # UNCOMMUN

    ("DOUBLE TIR", "+2 projectiles",
     "Uncommun", "Projectile", "projectiles", 2),

    ("FORCE", "+2 dégâts",
     "Uncommun", "Attaque", "damage", 2),

    ("SPRINT", "+70 vitesse",
     "Uncommun", "Vitesse", "move_speed", 70),

    ("GRAND COEUR", "+50 PV maximum",
     "Uncommun", "Soin", "max_hp", 50),

    ("SOINS RAPIDES", "+50 PV",
     "Uncommun", "Soin", "heal", 50),

    ("BLINDAGE", "+2 boucliers",
     "Uncommun", "Défense", "shield", 2),

    ("CADENCE", "Tire plus rapidement",
     "Uncommun", "Projectile", "fire_reduce", 0.035),

    ("BALLE LOURDE", "+5 taille de balle",
     "Uncommun", "Attaque", "bullet_size", 5),

    ("RÉGÉNÉRATION II", "+2 régénération/s",
     "Uncommun", "Soin", "regen", 2),

    ("ŒIL VIF", "+10% critique",
     "Uncommun", "Critique", "crit", 0.10),


    # RARE

    ("TRIPLE SALVE", "+3 projectiles",
     "Rare", "Projectile", "projectiles", 3),

    ("PUISSANCE", "+4 dégâts",
     "Rare", "Attaque", "damage", 4),

    ("TURBO", "+120 vitesse",
     "Rare", "Vitesse", "move_speed", 120),

    ("ARMURE", "+100 PV maximum",
     "Rare", "Défense", "max_hp", 100),

    ("MÉDECIN", "+100 PV",
     "Rare", "Soin", "heal", 100),

    ("FORTERESSE", "+3 boucliers",
     "Rare", "Défense", "shield", 3),

    ("MITRAILLETTE", "Tire beaucoup plus vite",
     "Rare", "Projectile", "fire_reduce", 0.065),

    ("BALLE PERFORANTE", "+3 dégâts",
     "Rare", "Attaque", "damage", 3),

    ("VAMPIRISME", "+2 PV par impact",
     "Rare", "Soin", "lifesteal", 2),

    ("CRITIQUE", "+15% critique",
     "Rare", "Critique", "crit", 0.15),


    # ÉPIQUE

    ("TEMPÊTE", "+5 projectiles",
     "Épique", "Projectile", "projectiles", 5),

    ("BERSERK", "+8 dégâts",
     "Épique", "Attaque", "damage", 8),

    ("ÉCLAIR", "+200 vitesse",
     "Épique", "Vitesse", "move_speed", 200),

    ("TITAN", "+200 PV maximum",
     "Épique", "Défense", "max_hp", 200),

    ("RÉGÉNÉRATION III", "+5 PV/s",
     "Épique", "Soin", "regen", 5),

    ("BOUCLIER TITAN", "+5 boucliers",
     "Épique", "Défense", "shield", 5),

    ("FUSIL RAPIDE", "Cadence énorme",
     "Épique", "Projectile", "fire_reduce", 0.10),

    ("CRITIQUE PUISSANT", "+50% multiplicateur critique",
     "Épique", "Critique", "crit_damage", 0.5),

    ("EXPLOSION", "+15% chance d'explosion",
     "Épique", "Explosion", "explosion", 0.15),

    ("BOMBARDEMENT", "Explosions +5 dégâts",
     "Épique", "Explosion", "explosion_damage", 5),


    # LÉGENDAIRE

    ("PLUIE DE BALLES", "+8 projectiles",
     "Légendaire", "Projectile", "projectiles", 8),

    ("DESTRUCTEUR", "+15 dégâts",
     "Légendaire", "Attaque", "damage", 15),

    ("FUSÉE", "+350 vitesse",
     "Légendaire", "Vitesse", "move_speed", 350),

    ("IMMORTEL", "+400 PV maximum",
     "Légendaire", "Défense", "max_hp", 400),

    ("RÉGÉNÉRATION IV", "+10 PV/s",
     "Légendaire", "Soin", "regen", 10),

    ("MUR D'ÉNERGIE", "+8 boucliers",
     "Légendaire", "Défense", "shield", 8),

    ("CRITIQUE ABSOLU", "+25% critique",
     "Légendaire", "Critique", "crit", 0.25),

    ("CHAOS", "+30% explosion",
     "Légendaire", "Explosion", "explosion", 0.30),

    ("GROS BANG", "+12 dégâts d'explosion",
     "Légendaire", "Explosion", "explosion_damage", 12),

    ("VAMPIRE ROUGE", "+8 PV par impact",
     "Légendaire", "Soin", "lifesteal", 8),


    # MYTHIQUE

    ("ARSENAL DIVIN", "+12 projectiles",
     "Mythique", "Projectile", "projectiles", 12),

    ("LAME COSMIQUE", "+30 dégâts",
     "Mythique", "Attaque", "damage", 30),

    ("ÉCLAIR DIVIN", "+500 vitesse",
     "Mythique", "Vitesse", "move_speed", 500),

    ("COEUR COSMIQUE", "+800 PV maximum",
     "Mythique", "Défense", "max_hp", 800),

    ("RÉSURRECTION", "+20 régénération/s",
     "Mythique", "Soin", "regen", 20),

    ("BARRIÈRE COSMIQUE", "+12 boucliers",
     "Mythique", "Défense", "shield", 12),

    ("CRITIQUE DIVIN", "+40% critique",
     "Mythique", "Critique", "crit", 0.40),

    ("NOVA", "+50% explosion",
     "Mythique", "Explosion", "explosion", 0.50),

    ("SUPERNOVA", "Explosions +30 dégâts",
     "Mythique", "Explosion", "explosion_damage", 30),

    ("VAMPIRISME DIVIN", "+20 PV par impact",
     "Mythique", "Soin", "lifesteal", 20),


    # DIVIN

    ("GOD MODE", "+50 dégâts",
     "Divin", "Spécial", "damage", 50),

    ("OMNIPOTENCE", "+20 projectiles",
     "Divin", "Projectile", "projectiles", 20),

    ("TEMPS ZERO", "Cadence maximale",
     "Divin", "Spécial", "fire_reduce", 0.18),

    ("ÉTERNITÉ", "+1500 PV maximum",
     "Divin", "Défense", "max_hp", 1500),

    ("SANG DIVIN", "+50 régénération/s",
     "Divin", "Soin", "regen", 50),

    ("ARMURE DIVINE", "+20 boucliers",
     "Divin", "Défense", "shield", 20),

    ("CRITIQUE INFINI", "+50% critique",
     "Divin", "Critique", "crit", 0.50),

    ("BIG BANG", "Explosions +75 dégâts",
     "Divin", "Explosion", "explosion_damage", 75),
]


# ============================================================
# 50 MONSTRES
# ============================================================

MONSTERS = [

    ("Slime", 1, 1, 60, 13, "#22c55e", 15, "normal"),
    ("Drone", 1, 0.8, 95, 11, "#38bdf8", 12, "normal"),
    ("Crawler", 1, 1.2, 75, 14, "#ef4444", 15, "normal"),
    ("Gobelin", 1, 1.4, 70, 15, "#84cc16", 15, "normal"),

    ("Chasseur", 2, 1, 120, 11, "#f97316", 16, "fast"),
    ("Araignée", 2, 1.1, 110, 12, "#e11d48", 16, "fast"),
    ("Goule", 2, 1.7, 55, 17, "#94a3b8", 18, "normal"),

    ("Squelette", 3, 1.5, 80, 15, "#e5e7eb", 18, "normal"),
    ("Mante", 3, 1.3, 130, 12, "#16a34a", 18, "fast"),
    ("Blob", 3, 2.2, 45, 20, "#06b6d4", 20, "tank"),

    ("Orbe", 4, 1.8, 100, 14, "#8b5cf6", 20, "normal"),
    ("Brute", 4, 3, 42, 23, "#7c3aed", 25, "tank"),

    ("Faucheur", 5, 2, 100, 16, "#dc2626", 22, "normal"),
    ("Fantôme", 5, 1.4, 115, 13, "#c4b5fd", 18, "ghost"),
    ("Scarabée", 5, 1.2, 145, 10, "#eab308", 18, "fast"),

    ("Molosse", 6, 2.5, 125, 17, "#b91c1c", 25, "fast"),
    ("Sentinelle", 6, 4, 50, 25, "#6366f1", 30, "tank"),

    ("Mage noir", 7, 2.2, 75, 16, "#4c1d95", 25, "normal"),
    ("Araignée reine", 7, 3.5, 90, 22, "#be123c", 30, "tank"),

    ("Démon", 8, 3.2, 100, 20, "#991b1b", 30, "normal"),
    ("Assassin", 8, 1.7, 170, 11, "#111827", 25, "fast"),

    ("Golem", 9, 5.5, 40, 28, "#78716c", 35, "tank"),
    ("Vampire", 9, 3, 105, 18, "#7f1d1d", 30, "vampire"),

    ("Wyrm", 10, 4, 115, 19, "#059669", 35, "fast"),

    ("Chevalier maudit", 11, 5, 75, 22, "#334155", 40, "tank"),
    ("Liche", 12, 4.5, 80, 18, "#9333ea", 35, "regen"),

    ("Berserker", 13, 3.5, 155, 17, "#dc2626", 40, "berserk"),
    ("Mimique", 14, 4, 90, 19, "#ca8a04", 35, "normal"),

    ("Titan", 15, 8, 38, 34, "#475569", 50, "tank"),
    ("Draugr", 16, 6, 70, 22, "#64748b", 45, "normal"),

    ("Pyromancien", 17, 5, 85, 18, "#ea580c", 40, "fire"),
    ("Cryomancien", 18, 5.5, 70, 19, "#0ea5e9", 40, "slow"),

    ("Foudre", 19, 4.5, 150, 15, "#facc15", 45, "fast"),
    ("Nécroïde", 20, 6.5, 80, 20, "#581c87", 45, "regen"),

    ("Colosse", 21, 10, 35, 38, "#52525b", 60, "tank"),
    ("Ravageur", 22, 7, 120, 24, "#b91c1c", 55, "fast"),

    ("Spectre", 23, 6, 130, 17, "#a78bfa", 45, "ghost"),
    ("Hydre", 24, 9, 75, 30, "#15803d", 60, "regen"),

    ("Dragonnet", 25, 8, 145, 23, "#ea580c", 65, "fire"),

    ("Seigneur noir", 27, 11, 90, 30, "#1e1b4b", 70, "tank"),
    ("Léviathan", 28, 13, 60, 36, "#0369a1", 80, "slow"),

    ("Dévoreur", 29, 12, 65, 32, "#7f1d1d", 75, "vampire"),

    ("Ange déchu", 31, 10, 135, 24, "#4338ca", 70, "ghost"),

    ("Machine de guerre", 33, 14, 55, 35, "#475569", 85, "tank"),

    ("Abomination", 35, 16, 70, 37, "#4d7c0f", 90, "regen"),

    ("Démon ancien", 38, 18, 105, 31, "#7f1d1d", 100, "fire"),

    ("Géant astral", 41, 22, 45, 43, "#0f766e", 120, "tank"),

    ("Voidling", 44, 20, 125, 27, "#312e81", 110, "ghost"),

    ("Annihilateur", 47, 26, 80, 40, "#be123c", 140, "tank"),

    ("Apocalypse", 50, 35, 100, 45, "#450a0a", 170, "fire")
]


# ============================================================
# JEU
# ============================================================

class NeonArena:

    def __init__(self, root):

        self.root = root
        self.root.title("NEON ARENA")
        self.root.configure(bg=BG)

        self.root.minsize(640, 420)
        self.root.resizable(True, True)

        screen_w = root.winfo_screenwidth()
        screen_h = root.winfo_screenheight()

        window_w = min(
            WIDTH,
            max(640, screen_w - 40)
        )

        window_h = min(
            HEIGHT,
            max(420, screen_h - 80)
        )

        self.root.geometry(
            f"{window_w}x{window_h}"
        )

        self.canvas = tk.Canvas(
            root,
            width=WIDTH,
            height=HEIGHT,
            bg=BG,
            highlightthickness=0
        )

        self.canvas.pack(
            fill="both",
            expand=True
        )

        # ADAPTATION ÉCRAN

        self.sx = 1
        self.sy = 1

        self.root.bind(
            "<Configure>",
            self.resize
        )

        # INPUT

        self.keys = set()

        self.mouse_x = WIDTH // 2
        self.mouse_y = HEIGHT // 2

        self.mouse_down = False

        self.root.bind(
            "<KeyPress>",
            self.key_down
        )

        self.root.bind(
            "<KeyRelease>",
            self.key_up
        )

        self.canvas.bind(
            "<Motion>",
            self.mouse_move
        )

        self.canvas.bind(
            "<ButtonPress-1>",
            self.mouse_press
        )

        self.canvas.bind(
            "<ButtonRelease-1>",
            self.mouse_release
        )

        self.canvas.bind(
            "<MouseWheel>",
            self.mouse_wheel
        )

        # OBJETS

        self.player = None

        self.enemies = []
        self.bullets = []
        self.enemy_bullets = []
        self.particles = []

        self.boss = None

        # SCORE

        self.score = 0
        self.high_score = 0
        self.coins = 0

        # VAGUES

        self.wave = 1
        self.wave_kills = 0
        self.wave_total = 8

        self.max_alive = 5

        # STATS

        self.max_hp = 100
        self.hp = 100

        self.damage = 1
        self.projectiles = 1

        self.fire_rate = 0.18
        self.move_speed = 300

        self.bullet_speed = 700
        self.bullet_size = 4

        self.crit = 0.05
        self.crit_mult = 2

        self.lifesteal = 0

        self.shield = 0

        self.explosion = 0
        self.explosion_damage = 2

        self.regen = 0

        # SPÉCIAL

        self.eren_stage = 0

        self.nuke_timer = 0
        self.lightning_timer = 0

        self.boost_until = 0

        # ÉTAT

        self.state = "menu"

        self.running = False
        self.game_over = False

        self.wave_transition = False

        self.choices = []

        self.enc_scroll = 0

        self.last_time = time.time()
        self.last_shot = 0
        self.spawn_timer = 0

        self.show_menu()


    # ========================================================
    # RESPONSIVE
    # ========================================================

    def resize(self, event):

        if event.widget is self.root:

            self.sx = max(
                640,
                event.width
            ) / WIDTH

            self.sy = max(
                420,
                event.height
            ) / HEIGHT

            self.draw()


    def world(self, x, y):

        return (
            x / self.sx,
            y / self.sy
        )


    def fit(self):

        self.canvas.scale(
            "all",
            0,
            0,
            self.sx,
            self.sy
        )


    # ========================================================
    # FOND
    # ========================================================

    def background(self):

        for x in range(
            0,
            WIDTH,
            50
        ):

            self.canvas.create_line(
                x,
                0,
                x,
                HEIGHT,
                fill=GRID
            )

        for y in range(
            0,
            HEIGHT,
            50
        ):

            self.canvas.create_line(
                0,
                y,
                WIDTH,
                y,
                fill=GRID
            )


    # ========================================================
    # BOUTON
    # ========================================================

    def button(
        self,
        x1,
        y1,
        x2,
        y2,
        text,
        color,
        size=11
    ):

        self.canvas.create_rectangle(
            x1,
            y1,
            x2,
            y2,
            fill="#111827",
            outline=color,
            width=2
        )

        self.canvas.create_text(
            (x1 + x2) // 2,
            (y1 + y2) // 2,
            text=text,
            font=(
                "Arial",
                size,
                "bold"
            ),
            fill=TEXT
        )


    # ========================================================
    # MENU
    # ========================================================

    def show_menu(self):

        self.state = "menu"

        self.draw()


    def menu_draw(self):

        self.canvas.create_text(
            WIDTH // 2,
            175,
            text="NEON",
            font=(
                "Arial",
                64,
                "bold"
            ),
            fill=PLAYER_COLOR
        )

        self.canvas.create_text(
            WIDTH // 2,
            245,
            text="ARENA",
            font=(
                "Arial",
                64,
                "bold"
            ),
            fill=PURPLE
        )

        self.canvas.create_text(
            WIDTH // 2,
            315,
            text="SURVIVE • SHOOT • UPGRADE",
            font=(
                "Arial",
                13,
                "bold"
            ),
            fill=MUTED
        )

        self.button(
            360,
            365,
            640,
            425,
            "JOUER",
            PLAYER_COLOR,
            16
        )

        self.button(
            360,
            445,
            640,
            500,
            "COMPÉTENCES / MONSTRES",
            PURPLE,
            10
        )

        self.canvas.create_text(
            WIDTH // 2,
            535,
            text=(
                "ZQSD / FLÈCHES : déplacer"
                "   •   "
                "CLIC GAUCHE : tirer"
            ),
            font=(
                "Arial",
                10
            ),
            fill=MUTED
        )


    # ========================================================
    # DÉMARRER
    # ========================================================

    def start_game(self):

        self.score = 0
        self.coins = 0

        self.wave = 1
        self.wave_kills = 0
        self.wave_total = self.required()

        self.max_alive = 5

        self.max_hp = 100
        self.hp = 100

        self.damage = 1
        self.projectiles = 1

        self.fire_rate = 0.18
        self.move_speed = 300

        self.bullet_speed = 700
        self.bullet_size = 4

        self.crit = 0.05
        self.crit_mult = 2

        self.lifesteal = 0
        self.shield = 0

        self.explosion = 0
        self.explosion_damage = 2

        self.regen = 0

        self.boost_until = 0

        self.eren_stage = 0

        self.nuke_timer = 0
        self.lightning_timer = 0

        self.enemies.clear()
        self.bullets.clear()
        self.enemy_bullets.clear()
        self.particles.clear()

        self.boss = None

        self.player = {
            "x": WIDTH // 2,
            "y": 360,
            "r": 16
        }

        self.spawn_timer = 0

        self.running = True
        self.game_over = False
        self.wave_transition = False
        self.state = "game"

        self.last_time = time.time()

        self.game_loop()


    # ========================================================
    # VAGUES
    # ========================================================

    def required(self):

        if self.wave in (100, 120):

            return 1

        return (
            8
            +
            (self.wave - 1) * 4
            +
            (
                1
                if self.wave % 10 == 0
                else 0
            )
        )


    def available(self):

        return [
            monster
            for monster in MONSTERS
            if monster[1] <= self.wave
        ]


    def unlocked_rarities(self):

        if self.wave >= 40:

            return list(
                RARITY_COLORS
            )

        if self.wave >= 30:

            return list(
                RARITY_COLORS
            )[:6]

        if self.wave >= 20:

            return list(
                RARITY_COLORS
            )[:5]

        if self.wave >= 10:

            return list(
                RARITY_COLORS
            )[:4]

        if self.wave >= 5:

            return list(
                RARITY_COLORS
            )[:3]

        if self.wave >= 2:

            return list(
                RARITY_COLORS
            )[:2]

        return [
            "Commun"
        ]


    # ========================================================
    # INPUT
    # ========================================================

    def key_down(self, event):

        key = event.keysym.lower()

        self.keys.add(key)

        if (
            self.game_over
            and key == "return"
        ):

            self.start_game()

            return

        # BOUTIQUE

        if (
            key == "b"
            and self.state == "game"
            and not self.wave_transition
        ):

            self.state = "shop"

            self.mouse_down = False

            self.draw()

            return

        # ESCAPE

        if key == "escape":

            if self.state == "shop":

                self.state = "game"

                self.last_time = time.time()

                self.draw()

            elif self.state == "encyclopedia":

                self.state = "menu"

                self.draw()

            elif self.state == "menu":

                self.root.destroy()


    def key_up(self, event):

        self.keys.discard(
            event.keysym.lower()
        )


    def mouse_move(self, event):

        self.mouse_x, self.mouse_y = self.world(
            event.x,
            event.y
        )


    def mouse_release(self, event):

        self.mouse_down = False


    def mouse_wheel(self, event):

        if self.state != "encyclopedia":

            return

        amount = (
            event.delta // 120
            if event.delta
            else 0
        )

        maximum = max(
            0,
            len(UPGRADES) * 34
            +
            (len(MONSTERS) + 2) * 44
            -
            470
        )

        self.enc_scroll = max(
            0,
            min(
                maximum,
                self.enc_scroll -
                amount * 60
            )
        )

        self.draw()


    def mouse_press(self, event):

        x, y = self.world(
            event.x,
            event.y
        )

        # MENU

        if self.state == "menu":

            if (
                360 <= x <= 640
                and
                365 <= y <= 425
            ):

                self.start_game()

            elif (
                360 <= x <= 640
                and
                445 <= y <= 500
            ):

                self.state = "encyclopedia"

                self.enc_scroll = 0

                self.draw()

            return

        # ENCYCLOPÉDIE

        if self.state == "encyclopedia":

            return

        # BOUTIQUE

        if self.state == "shop":

            self.shop_click(
                x,
                y
            )

            return

        # COMPÉTENCE

        if self.wave_transition:

            self.choose_upgrade(
                x,
                y
            )

            return

        # JEU

        if self.state == "game":

            self.mouse_down = True


    # ========================================================
    # BOUCLE
    # ========================================================

    def game_loop(self):

        if not self.running:

            if self.game_over:

                self.draw()

            return

        now = time.time()

        dt = min(
            0.05,
            now - self.last_time
        )

        self.last_time = now

        self.update(dt)

        self.draw()

        self.root.after(
            16,
            self.game_loop
        )


    # ========================================================
    # UPDATE
    # ========================================================

    def update(self, dt):

        if self.state != "game":

            return

        if self.wave_transition:

            return

        self.player_update(dt)

        self.bullets_update(dt)

        self.enemies_update(dt)

        self.enemy_bullets_update(dt)

        self.particles_update(dt)

        self.nuke_timer = max(
            0,
            self.nuke_timer - dt
        )

        self.lightning_timer = max(
            0,
            self.lightning_timer - dt
        )

        if self.regen:

            self.hp = min(
                self.max_hp,
                self.hp +
                self.regen * dt
            )

        # VAGUES 100 ET 120

        if self.wave in (100, 120):

            if (
                not self.enemies
                and
                self.boss is None
            ):

                if self.wave_kills >= 1:

                    self.start_upgrade()

                else:

                    self.spawn_special()

            return

        # SPAWN NORMAL

        self.spawn_timer += dt

        regular_target = (
            self.wave_total
            -
            (
                1
                if self.wave % 10 == 0
                else 0
            )
        )

        if (
            self.wave_kills <
            regular_target
            and
            len(self.enemies) <
            self.max_alive
            and
            self.spawn_timer >=
            self.spawn_delay
        ):

            self.spawn_enemy()

            self.spawn_timer = 0

        # BOSS TOUS LES 10

        if (
            self.wave % 10 == 0
            and
            self.wave_kills >=
            regular_target
            and
            self.boss is None
            and
            not self.enemies
        ):

            self.spawn_boss()

        # FIN

        if (
            self.wave_kills >=
            self.wave_total
            and
            not self.enemies
            and
            self.boss is None
        ):

            self.start_upgrade()


    # ========================================================
    # JOUEUR
    # ========================================================

    def player_update(self, dt):

        dx = (
            ("d" in self.keys or "right" in self.keys)
            -
            ("q" in self.keys or "left" in self.keys)
        )

        dy = (
            ("s" in self.keys or "down" in self.keys)
            -
            ("z" in self.keys or "up" in self.keys)
        )

        if dx or dy:

            length = math.hypot(
                dx,
                dy
            )

            dx /= length
            dy /= length

            self.player["x"] += (
                dx *
                self.move_speed *
                dt
            )

            self.player["y"] += (
                dy *
                self.move_speed *
                dt
            )

        r = self.player["r"]

        self.player["x"] = max(
            r,
            min(
                WIDTH - r,
                self.player["x"]
            )
        )

        self.player["y"] = max(
            90 + r,
            min(
                HEIGHT - r,
                self.player["y"]
            )
        )

        if self.mouse_down:

            self.shoot()


    # ========================================================
    # TIR
    # ========================================================

    def shoot(self):

        now = time.time()

        if (
            now -
            self.last_shot
            <
            self.fire_rate
        ):

            return

        self.last_shot = now

        dx = (
            self.mouse_x -
            self.player["x"]
        )

        dy = (
            self.mouse_y -
            self.player["y"]
        )

        distance = math.hypot(
            dx,
            dy
        )

        if not distance:

            return

        base = math.atan2(
            dy,
            dx
        )

        count = max(
            1,
            int(self.projectiles)
        )

        spread = math.radians(9)

        start = (
            base -
            spread *
            (count - 1) /
            2
        )

        for i in range(count):

            angle = (
                start +
                i * spread
            )

            damage = self.damage

            critical = (
                random.random()
                <
                self.crit
            )

            if critical:

                damage = int(
                    damage *
                    self.crit_mult
                )

            if now < self.boost_until:

                damage *= 2

            self.bullets.append({

                "x":
                    self.player["x"],

                "y":
                    self.player["y"],

                "dx":
                    math.cos(angle),

                "dy":
                    math.sin(angle),

                "speed":
                    self.bullet_speed,

                "damage":
                    damage,

                "life":
                    1.5,

                "critical":
                    critical
            })


    # ========================================================
    # BALLES
    # ========================================================

    def bullets_update(self, dt):

        for bullet in self.bullets[:]:

            bullet["x"] += (
                bullet["dx"] *
                bullet["speed"] *
                dt
            )

            bullet["y"] += (
                bullet["dy"] *
                bullet["speed"] *
                dt
            )

            bullet["life"] -= dt

            if (
                bullet["life"] <= 0
                or
                not (
                    0 <=
                    bullet["x"]
                    <= WIDTH
                )
                or
                not (
                    70 <=
                    bullet["y"]
                    <= HEIGHT
                )
            ):

                if bullet in self.bullets:

                    self.bullets.remove(
                        bullet
                    )

                continue

            for enemy in self.enemies[:]:

                distance = math.hypot(
                    bullet["x"] -
                    enemy["x"],
                    bullet["y"] -
                    enemy["y"]
                )

                if distance < (
                    enemy["r"] +
                    self.bullet_size
                ):

                    if bullet in self.bullets:

                        self.bullets.remove(
                            bullet
                        )

                    enemy["hp"] -= (
                        bullet["damage"]
                    )

                    if self.lifesteal:

                        self.hp = min(
                            self.max_hp,
                            self.hp +
                            self.lifesteal
                        )

                    if (
                        random.random()
                        <
                        self.explosion
                    ):

                        self.explode(
                            enemy["x"],
                            enemy["y"],
                            enemy
                        )

                    if enemy["hp"] <= 0:

                        self.kill(enemy)

                    break


    # ========================================================
    # SPAWN MONSTRE
    # ========================================================

    def spawn_enemy(self):

        if self.wave in (100, 120):

            return

        monster = random.choice(
            self.available()
        )

        (
            name,
            min_wave,
            hp_base,
            speed,
            radius,
            color,
            contact,
            special
        ) = monster

        side = random.randrange(4)

        if side == 0:

            x = random.randint(
                20,
                980
            )

            y = 90

        elif side == 1:

            x = 980

            y = random.randint(
                90,
                630
            )

        elif side == 2:

            x = random.randint(
                20,
                980
            )

            y = 630

        else:

            x = 20

            y = random.randint(
                90,
                630
            )

        hp = max(
            1,
            int(
                hp_base *
                (
                    1 +
                    max(
                        0,
                        self.wave -
                        min_wave
                    )
                    *
                    0.08
                )
            )
        )

        speed *= (
            1 +
            max(
                0,
                self.wave - 10
            )
            *
            0.01
        )

        self.enemies.append({

            "x": x,
            "y": y,

            "r": radius,

            "speed": speed,

            "hp": hp,
            "max_hp": hp,

            "color": color,

            "type": name,

            "special": special,

            "contact": contact,

            "cooldown": 0
        })


    # ========================================================
    # BOSS NORMAL
    # ========================================================

    def spawn_boss(self):

        hp = (
            500 +
            self.wave *
            100
        )

        x = random.choice(
            (
                35,
                965
            )
        )

        y = random.randint(
            120,
            610
        )

        self.boss = {

            "x": x,
            "y": y,

            "r": 38,

            "speed":
                35 +
                self.wave *
                0.7,

            "hp": hp,
            "max_hp": hp,

            "color": PURPLE,

            "type": "BOSS",

            "special": "boss",

            "contact": 50,

            "cooldown": 0
        }

        self.enemies.append(
            self.boss
        )


    # ========================================================
    # BOSS SPÉCIAUX
    # ========================================================

    def spawn_special(self):

        # VAGUE 100
        # KOKUSHIBO UNIQUEMENT

        if self.wave == 100:

            hp = 18000

            self.boss = {

                "x": 850,
                "y": 330,

                "r": 48,

                "speed": 75,

                "hp": hp,
                "max_hp": hp,

                "color": "#7c3aed",

                "type": "KOKUSHIBO",

                "special":
                    "kokushibo",

                "contact": 90,

                "cooldown": 0,

                "teleport": 4
            }

            self.enemies.append(
                self.boss
            )

            return

        # VAGUE 120
        # EREN TITAN ASSAILLANT

        self.eren_stage = 1

        self.boss = {

            "x": 850,
            "y": 330,

            "r": 42,

            "speed": 80,

            "hp": 24000,
            "max_hp": 24000,

            "color": "#7f1d1d",

            "type":
                "EREN - TITAN ASSAILLANT",

            "special":
                "eren_attack",

            "contact": 110,

            "cooldown": 1.2
        }

        self.enemies.append(
            self.boss
        )

        # ÉCLAIR DE TRANSFORMATION

        self.lightning_timer = 1.5


    # ========================================================
    # PROJECTILES ENNEMIS
    # ========================================================

    def enemy_shot(
        self,
        enemy,
        angle,
        speed,
        damage,
        color,
        radius=8,
        kind="normal"
    ):

        self.enemy_bullets.append({

            "x":
                enemy["x"],

            "y":
                enemy["y"],

            "dx":
                math.cos(angle),

            "dy":
                math.sin(angle),

            "speed":
                speed,

            "damage":
                damage,

            "color":
                color,

            "r":
                radius,

            "life":
                4,

            "kind":
                kind
        })


    # ========================================================
    # UPDATE ENNEMIS
    # ========================================================

    def enemies_update(self, dt):

        for enemy in self.enemies[:]:

            enemy["cooldown"] = max(
                0,
                enemy.get(
                    "cooldown",
                    0
                ) -
                dt
            )

            special = enemy.get(
                "special"
            )

            dx = (
                self.player["x"] -
                enemy["x"]
            )

            dy = (
                self.player["y"] -
                enemy["y"]
            )

            distance = math.hypot(
                dx,
                dy
            )

            if not distance:

                distance = 1

            dx /= distance
            dy /= distance

            speed = enemy["speed"]

            # LENT

            if special == "slow":

                speed *= 0.7

            # BERSERKER

            if (
                special == "berserk"
                and
                enemy["hp"]
                <
                enemy["max_hp"] *
                0.4
            ):

                speed *= 1.5

            # COLOSSAL

            if special == "eren_colossal":

                speed *= 0.8

            enemy["x"] += (
                dx *
                speed *
                dt
            )

            enemy["y"] += (
                dy *
                speed *
                dt
            )

            # RÉGÉNÉRATION

            if (
                special == "regen"
                and
                enemy["hp"]
                <
                enemy["max_hp"]
            ):

                enemy["hp"] = min(
                    enemy["max_hp"],
                    enemy["hp"] +
                    dt
                )

            # =================================================
            # KOKUSHIBO
            # =================================================

            if special == "kokushibo":

                enemy["hp"] = min(
                    enemy["max_hp"],
                    enemy["hp"] +
                    8 * dt
                )

                enemy["teleport"] -= dt

                if enemy["cooldown"] <= 0:

                    angle = math.atan2(
                        self.player["y"] -
                        enemy["y"],
                        self.player["x"] -
                        enemy["x"]
                    )

                    # Lames lunaires

                    for offset in (
                        -0.34,
                        -0.17,
                        0,
                        0.17,
                        0.34
                    ):

                        self.enemy_shot(
                            enemy,
                            angle + offset,
                            390,
                            24,
                            "#a855f7",
                            9,
                            "moon"
                        )

                    enemy["cooldown"] = 1.25

                # Téléportation

                if enemy["teleport"] <= 0:

                    enemy["x"] = random.choice(
                        (
                            70,
                            930
                        )
                    )

                    enemy["y"] = random.randint(
                        110,
                        600
                    )

                    enemy["teleport"] = 4

            # =================================================
            # EREN TITAN ASSAILLANT
            # =================================================

            if (
                special ==
                "eren_attack"
                and
                enemy["cooldown"] <= 0
            ):

                angle = math.atan2(
                    self.player["y"] -
                    enemy["y"],
                    self.player["x"] -
                    enemy["x"]
                )

                for offset in (
                    -0.12,
                    0,
                    0.12
                ):

                    self.enemy_shot(
                        enemy,
                        angle + offset,
                        300,
                        35,
                        "#ef4444",
                        10,
                        "titan"
                    )

                enemy["cooldown"] = 2.2

            # =================================================
            # EREN COLOSSAL
            # =================================================

            if (
                special ==
                "eren_colossal"
                and
                enemy["cooldown"] <= 0
            ):

                distance_to_player = math.hypot(
                    self.player["x"] -
                    enemy["x"],
                    self.player["y"] -
                    enemy["y"]
                )

                if distance_to_player < 190:

                    self.take_damage(
                        55
                    )

                angle = math.atan2(
                    self.player["y"] -
                    enemy["y"],
                    self.player["x"] -
                    enemy["x"]
                )

                for offset in (
                    -0.22,
                    0,
                    0.22
                ):

                    self.enemy_shot(
                        enemy,
                        angle + offset,
                        260,
                        45,
                        "#fb923c",
                        12,
                        "steam"
                    )

                enemy["cooldown"] = 1.4

            # =================================================
            # COLLISION JOUEUR
            # =================================================

            distance = math.hypot(
                self.player["x"] -
                enemy["x"],
                self.player["y"] -
                enemy["y"]
            )

            if distance < (
                self.player["r"] +
                enemy["r"]
            ):

                # UN CONTACT NE COMPTE JAMAIS
                # COMME UN KILL

                if special in (
                    "boss",
                    "kokushibo",
                    "eren_attack",
                    "eren_colossal"
                ):

                    self.take_damage(
                        enemy["contact"]
                    )

                    enemy["x"] -= (
                        dx *
                        100
                    )

                    enemy["y"] -= (
                        dy *
                        100
                    )

                else:

                    if enemy in self.enemies:

                        self.enemies.remove(
                            enemy
                        )

                    self.take_damage(
                        enemy["contact"]
                    )

                    regular_target = (
                        self.wave_total
                        -
                        (
                            1
                            if self.wave % 10 == 0
                            else 0
                        )
                    )

                    if (
                        self.wave_kills
                        <
                        regular_target
                    ):

                        self.spawn_enemy()


    # ========================================================
    # PROJECTILES ENNEMIS
    # ========================================================

    def enemy_bullets_update(self, dt):

        for bullet in self.enemy_bullets[:]:

            bullet["x"] += (
                bullet["dx"] *
                bullet["speed"] *
                dt
            )

            bullet["y"] += (
                bullet["dy"] *
                bullet["speed"] *
                dt
            )

            bullet["life"] -= dt

            if (
                bullet["life"] <= 0
                or
                not (
                    0 <=
                    bullet["x"]
                    <= WIDTH
                )
                or
                not (
                    70 <=
                    bullet["y"]
                    <= HEIGHT
                )
            ):

                self.enemy_bullets.remove(
                    bullet
                )

                continue

            distance = math.hypot(
                bullet["x"] -
                self.player["x"],
                bullet["y"] -
                self.player["y"]
            )

            if distance < (
                bullet["r"] +
                self.player["r"]
            ):

                self.take_damage(
                    bullet["damage"]
                )

                self.create_explosion(
                    bullet["x"],
                    bullet["y"],
                    bullet["color"]
                )

                if bullet in self.enemy_bullets:

                    self.enemy_bullets.remove(
                        bullet
                    )


    # ========================================================
    # DÉGÂTS
    # ========================================================

    def take_damage(self, amount):

        if self.shield > 0:

            self.shield -= 1

            self.create_explosion(
                self.player["x"],
                self.player["y"],
                CYAN
            )

            return

        self.hp -= amount

        if self.hp <= 0:

            self.hp = 0

            self.end_game()


    # ========================================================
    # EXPLOSION
    # ========================================================

    def explode(
        self,
        x,
        y,
        source=None
    ):

        for enemy in self.enemies[:]:

            if enemy is source:

                continue

            distance = math.hypot(
                enemy["x"] - x,
                enemy["y"] - y
            )

            if distance <= 70:

                enemy["hp"] -= (
                    self.explosion_damage
                )

                if enemy["hp"] <= 0:

                    self.kill(enemy)

        self.create_explosion(
            x,
            y,
            ORANGE
        )


    def create_explosion(
        self,
        x,
        y,
        color
    ):

        for _ in range(16):

            angle = random.random() * math.tau

            speed = random.uniform(
                40,
                180
            )

            self.particles.append({

                "x": x,
                "y": y,

                "dx":
                    math.cos(angle)
                    *
                    speed,

                "dy":
                    math.sin(angle)
                    *
                    speed,

                "life":
                    0.5,

                "color":
                    color
            })


    # ========================================================
    # MORT D'UN MONSTRE
    # ========================================================

    def kill(self, enemy):

        if enemy not in self.enemies:

            return

        special = enemy.get(
            "special"
        )

        # ====================================================
        # PREMIÈRE MORT D'EREN
        # ====================================================

        if special == "eren_attack":

            self.create_explosion(
                enemy["x"],
                enemy["y"],
                enemy["color"]
            )

            self.transform_eren()

            return

        # ====================================================
        # MORT NORMALE
        # ====================================================

        self.create_explosion(
            enemy["x"],
            enemy["y"],
            enemy["color"]
        )

        self.enemies.remove(
            enemy
        )

        is_boss = (
            special in (
                "boss",
                "kokushibo",
                "eren_colossal"
            )
        )

        if enemy is self.boss:

            self.boss = None

        elif is_boss:

            self.boss = None

        self.score += 1

        self.wave_kills += 1

        if is_boss:

            self.coins += 50

        else:

            self.coins += 3

        # REMPLACEMENT

        if (
            self.wave not in (
                100,
                120
            )
            and
            self.wave_kills <
            self.wave_total
            and
            len(self.enemies) <
            self.max_alive
        ):

            regular_target = (
                self.wave_total
                -
                (
                    1
                    if self.wave % 10 == 0
                    else 0
                )
            )

            if (
                self.wave_kills
                <
                regular_target
            ):

                self.spawn_enemy()


    # ========================================================
    # TRANSFORMATION EREN
    # ========================================================

    def nuclear_explosion(self):

        self.nuke_timer = 2.5

        for _ in range(140):

            angle = random.random() * math.tau

            speed = random.uniform(
                100,
                500
            )

            self.particles.append({

                "x":
                    WIDTH // 2,

                "y":
                    HEIGHT // 2,

                "dx":
                    math.cos(angle)
                    *
                    speed,

                "dy":
                    math.sin(angle)
                    *
                    speed,

                "life":
                    random.uniform(
                        0.7,
                        2.2
                    ),

                "color":
                    random.choice(
                        (
                            YELLOW,
                            ORANGE,
                            TEXT,
                            RED
                        )
                    )
            })

        # Explosion importante

        self.take_damage(
            max(
                1,
                int(
                    self.max_hp *
                    0.35
                )
            )
        )


    def transform_eren(self):

        if self.boss not in self.enemies:

            return

        # Bombe nucléaire à la première mort

        self.nuclear_explosion()

        # Éclair de transformation

        self.lightning_timer = 2

        self.eren_stage = 2

        self.boss["type"] = (
            "EREN - TITAN COLOSSAL"
        )

        self.boss["special"] = (
            "eren_colossal"
        )

        self.boss["color"] = (
            "#ef4444"
        )

        self.boss["r"] = 68

        self.boss["speed"] = 30

        self.boss["hp"] = 42000

        self.boss["max_hp"] = 42000

        self.boss["contact"] = 180

        self.boss["cooldown"] = 1

        self.boss["x"] = (
            WIDTH // 2
        )

        self.boss["y"] = (
            HEIGHT // 2
        )


    # ========================================================
    # PARTICULES
    # ========================================================

    def particles_update(self, dt):

        for particle in self.particles[:]:

            particle["x"] += (
                particle["dx"] *
                dt
            )

            particle["y"] += (
                particle["dy"] *
                dt
            )

            particle["life"] -= dt

            if particle["life"] <= 0:

                self.particles.remove(
                    particle
                )


    # ========================================================
    # FIN DE VAGUE
    # ========================================================

    def start_upgrade(self):

        self.wave_transition = True

        self.mouse_down = False

        pool = [
            upgrade
            for upgrade in UPGRADES
            if upgrade[2]
            in
            self.unlocked_rarities()
        ]

        self.choices = random.sample(
            pool,
            3
        )


    def choose_upgrade(
        self,
        x,
        y
    ):

        if not (
            120 <= x <= 880
            and
            245 <= y <= 480
        ):

            return

        for i, upgrade in enumerate(
            self.choices
        ):

            x1 = (
                120 +
                i * 260
            )

            x2 = x1 + 220

            if (
                x1 <= x <= x2
            ):

                self.apply_upgrade(
                    upgrade
                )

                self.wave += 1

                self.wave_kills = 0

                self.wave_total = (
                    self.required()
                )

                self.max_alive = min(
                    12,
                    5 +
                    self.wave // 5
                )

                self.enemies.clear()

                self.enemy_bullets.clear()

                self.boss = None

                self.spawn_timer = 0

                self.spawn_delay = max(
                    0.18,
                    1 -
                    self.wave *
                    0.018
                )

                self.wave_transition = False

                self.state = "game"

                self.last_time = time.time()

                return


    # ========================================================
    # APPLICATION COMPÉTENCE
    # ========================================================

    def apply_upgrade(
        self,
        upgrade
    ):

        (
            name,
            description,
            rarity,
            skill_class,
            effect,
            value
        ) = upgrade

        if effect == "projectiles":

            self.projectiles += value

        elif effect == "damage":

            self.damage += value

        elif effect == "move_speed":

            self.move_speed += value

        elif effect == "max_hp":

            self.max_hp += value

            self.hp += value

        elif effect == "heal":

            self.hp = min(
                self.max_hp,
                self.hp + value
            )

        elif effect == "shield":

            self.shield += value

        elif effect == "bullet_speed":

            self.bullet_speed += value

        elif effect == "bullet_size":

            self.bullet_size += value

        elif effect == "regen":

            self.regen += value

        elif effect == "crit":

            self.crit = min(
                0.95,
                self.crit + value
            )

        elif effect == "crit_damage":

            self.crit_mult += value

        elif effect == "lifesteal":

            self.lifesteal += value

        elif effect == "explosion":

            self.explosion = min(
                0.95,
                self.explosion + value
            )

        elif effect == "explosion_damage":

            self.explosion_damage += value

        elif effect == "fire_reduce":

            self.fire_rate = max(
                0.025,
                self.fire_rate - value
            )


    # ========================================================
    # BOUTIQUE
    # ========================================================

    def shop_click(
        self,
        x,
        y
    ):

        if (
            120 <= x <= 380
            and
            190 <= y <= 290
        ):

            self.buy(
                "heal",
                20
            )

        elif (
            400 <= x <= 600
            and
            190 <= y <= 290
        ):

            self.buy(
                "shield",
                30
            )

        elif (
            620 <= x <= 880
            and
            190 <= y <= 290
        ):

            self.buy(
                "power",
                50
            )

        elif (
            360 <= x <= 640
            and
            500 <= y <= 555
        ):

            self.state = "game"

            self.last_time = time.time()

            self.draw()


    def buy(
        self,
        item,
        price
    ):

        if self.coins < price:

            return

        self.coins -= price

        if item == "heal":

            self.hp = min(
                self.max_hp,
                self.hp + 40
            )

        elif item == "shield":

            self.shield += 1

        elif item == "power":

            self.boost_until = (
                time.time() +
                60
            )

        self.draw()


    # ========================================================
    # DESSIN
    # ========================================================

    def draw(self):

        self.canvas.delete(
            "all"
        )

        self.background()

        if self.state == "menu":

            self.menu_draw()

            self.fit()

            return

        if self.state == "encyclopedia":

            self.encyclopedia()

            self.fit()

            return

        if self.player:

            self.hud()

            self.draw_bullets()

            self.draw_enemies()

            self.draw_enemy_bullets()

            self.draw_particles()

            self.draw_player()

            self.crosshair()

        if self.state == "shop":

            self.shop_draw()

        if self.wave_transition:

            self.upgrade_draw()

        if self.nuke_timer > 0:

            self.nuke_draw()

        if self.lightning_timer > 0:

            self.lightning_draw()

        if self.game_over:

            self.gameover_draw()

        self.fit()


    # ========================================================
    # HUD
    # ========================================================

    def hud(self):

        self.canvas.create_rectangle(
            0,
            0,
            WIDTH,
            75,
            fill=PANEL,
            outline=""
        )

        self.canvas.create_text(
            20,
            20,
            text="NEON ARENA",
            anchor="w",
            font=(
                "Arial",
                17,
                "bold"
            ),
            fill=PLAYER_COLOR
        )

        self.canvas.create_text(
            20,
            48,
            text=(
                f"SCORE  {self.score}"
                f"   •   "
                f"💰 {self.coins}"
            ),
            anchor="w",
            font=(
                "Arial",
                10,
                "bold"
            ),
            fill=TEXT
        )

        self.canvas.create_text(
            WIDTH // 2,
            20,
            text=f"WAVE {self.wave}",
            font=(
                "Arial",
                14,
                "bold"
            ),
            fill=PURPLE
        )

        if self.wave == 100:

            wave_text = (
                "KOKUSHIBO • BOSS UNIQUE"
            )

        elif self.wave == 120:

            wave_text = (
                "EREN • BOSS UNIQUE"
            )

        else:

            wave_text = (
                f"{self.wave_kills}"
                f" / "
                f"{self.wave_total}"
                f" ennemis"
            )

        self.canvas.create_text(
            WIDTH // 2,
            48,
            text=wave_text,
            font=(
                "Arial",
                9
            ),
            fill=MUTED
        )

        # PV

        bar_x = WIDTH - 300
        bar_y = 15

        bar_w = 180
        bar_h = 16

        self.canvas.create_rectangle(
            bar_x,
            bar_y,
            bar_x + bar_w,
            bar_y + bar_h,
            fill="#1f2937",
            outline=""
        )

        ratio = max(
            0,
            self.hp /
            self.max_hp
        )

        self.canvas.create_rectangle(
            bar_x,
            bar_y,
            bar_x +
            bar_w *
            ratio,
            bar_y + bar_h,
            fill=(
                GREEN
                if ratio > 0.3
                else RED
            ),
            outline=""
        )

        self.canvas.create_text(
            bar_x +
            bar_w / 2,
            bar_y + 8,
            text=(
                f"{int(self.hp)} / "
                f"{self.max_hp}"
            ),
            font=(
                "Arial",
                8,
                "bold"
            ),
            fill=TEXT
        )

        self.canvas.create_text(
            WIDTH - 20,
            55,
            text=f"🛡 {self.shield}",
            anchor="e",
            font=(
                "Arial",
                10,
                "bold"
            ),
            fill=CYAN
        )


    # ========================================================
    # JOUEUR
    # ========================================================

    def draw_player(self):

        x = self.player["x"]
        y = self.player["y"]
        r = self.player["r"]

        if self.shield:

            self.canvas.create_oval(
                x - r - 7,
                y - r - 7,
                x + r + 7,
                y + r + 7,
                outline=CYAN,
                width=2
            )

        self.canvas.create_oval(
            x - r - 5,
            y - r - 5,
            x + r + 5,
            y + r + 5,
            outline="#164e63",
            width=2
        )

        self.canvas.create_oval(
            x - r,
            y - r,
            x + r,
            y + r,
            fill=PLAYER_COLOR,
            outline=""
        )


    # ========================================================
    # VISEUR
    # ========================================================

    def crosshair(self):

        x = self.mouse_x
        y = self.mouse_y

        self.canvas.create_oval(
            x - 10,
            y - 10,
            x + 10,
            y + 10,
            outline=PLAYER_COLOR
        )

        self.canvas.create_line(
            x - 16,
            y,
            x - 5,
            y,
            fill=PLAYER_COLOR
        )

        self.canvas.create_line(
            x + 5,
            y,
            x + 16,
            y,
            fill=PLAYER_COLOR
        )

        self.canvas.create_line(
            x,
            y - 16,
            x,
            y - 5,
            fill=PLAYER_COLOR
        )

        self.canvas.create_line(
            x,
            y + 5,
            x,
            y + 16,
            fill=PLAYER_COLOR
        )


    # ========================================================
    # BALLES
    # ========================================================

    def draw_bullets(self):

        for bullet in self.bullets:

            size = self.bullet_size

            color = (
                YELLOW
                if bullet["critical"]
                else BULLET_COLOR
            )

            self.canvas.create_oval(
                bullet["x"] - size,
                bullet["y"] - size,
                bullet["x"] + size,
                bullet["y"] + size,
                fill=color,
                outline=""
            )


    # ========================================================
    # PROJECTILES ENNEMIS
    # ========================================================

    def draw_enemy_bullets(self):

        for bullet in self.enemy_bullets:

            x = bullet["x"]
            y = bullet["y"]
            r = bullet["r"]

            if bullet["kind"] == "moon":

                self.canvas.create_arc(
                    x - 2 * r,
                    y - 2 * r,
                    x + 2 * r,
                    y + 2 * r,
                    start=35,
                    extent=110,
                    outline=bullet["color"],
                    width=3
                )

            else:

                self.canvas.create_oval(
                    x - r,
                    y - r,
                    x + r,
                    y + r,
                    fill=bullet["color"],
                    outline=""
                )


    # ========================================================
    # ENNEMIS
    # ========================================================

    def draw_enemies(self):

        for enemy in self.enemies:

            x = enemy["x"]
            y = enemy["y"]
            r = enemy["r"]

            special = enemy["special"]

            # KOKUSHIBO

            if special == "kokushibo":

                self.canvas.create_oval(
                    x - r - 8,
                    y - r - 8,
                    x + r + 8,
                    y + r + 8,
                    outline=RED,
                    width=3
                )

                self.canvas.create_oval(
                    x - r,
                    y - r,
                    x + r,
                    y + r,
                    fill="#4c1d95",
                    outline=""
                )

                # 6 yeux

                for ex, ey in (
                    (-16, -12),
                    (0, -16),
                    (16, -12),
                    (-16, 12),
                    (0, 16),
                    (16, 12)
                ):

                    self.canvas.create_oval(
                        x + ex - 4,
                        y + ey - 3,
                        x + ex + 4,
                        y + ey + 3,
                        fill=RED,
                        outline=""
                    )

                continue

            # EREN TITAN

            if special == "eren_attack":

                self.canvas.create_oval(
                    x - r - 6,
                    y - r - 6,
                    x + r + 6,
                    y + r + 6,
                    outline=RED,
                    width=3
                )

                self.canvas.create_oval(
                    x - r,
                    y - r,
                    x + r,
                    y + r,
                    fill="#7f1d1d",
                    outline=""
                )

                self.canvas.create_line(
                    x - 22,
                    y - 8,
                    x + 22,
                    y - 8,
                    fill=TEXT,
                    width=3
                )

                continue

            # EREN COLOSSAL

            if special == "eren_colossal":

                self.canvas.create_oval(
                    x - r - 10,
                    y - r - 10,
                    x + r + 10,
                    y + r + 10,
                    outline=ORANGE,
                    width=4
                )

                self.canvas.create_oval(
                    x - r,
                    y - r,
                    x + r,
                    y + r,
                    fill="#991b1b",
                    outline=""
                )

                self.canvas.create_line(
                    x - 30,
                    y - 15,
                    x + 30,
                    y - 15,
                    fill=TEXT,
                    width=4
                )

                continue

            # MONSTRES NORMAUX

            self.canvas.create_oval(
                x - r - 5,
                y - r - 5,
                x + r + 5,
                y + r + 5,
                outline="#3f1d2e",
                width=2
            )

            self.canvas.create_oval(
                x - r,
                y - r,
                x + r,
                y + r,
                fill=enemy["color"],
                outline=""
            )

            if enemy["max_hp"] > 1:

                width = r * 2

                ratio = max(
                    0,
                    enemy["hp"] /
                    enemy["max_hp"]
                )

                self.canvas.create_rectangle(
                    x - width / 2,
                    y - r - 9,
                    x + width / 2,
                    y - r - 5,
                    fill="#1f2937",
                    outline=""
                )

                self.canvas.create_rectangle(
                    x - width / 2,
                    y - r - 9,
                    x -
                    width / 2 +
                    width *
                    ratio,
                    y - r - 5,
                    fill=GREEN,
                    outline=""
                )

        # BARRE BOSS

        if self.boss in self.enemies:

            boss = self.boss

            bx = 180
            by = 82

            bw = 640
            bh = 18

            ratio = max(
                0,
                boss["hp"] /
                boss["max_hp"]
            )

            self.canvas.create_rectangle(
                bx,
                by,
                bx + bw,
                by + bh,
                fill="#1f2937",
                outline=PURPLE,
                width=1
            )

            boss_color = (
                RED
                if boss["special"]
                ==
                "eren_colossal"
                else PURPLE
            )

            self.canvas.create_rectangle(
                bx,
                by,
                bx +
                bw *
                ratio,
                by + bh,
                fill=boss_color,
                outline=""
            )

            self.canvas.create_text(
                WIDTH // 2,
                by + bh / 2,
                text=(
                    f"{boss['type']}  "
                    f"{int(boss['hp'])} / "
                    f"{int(boss['max_hp'])}"
                ),
                font=(
                    "Arial",
                    9,
                    "bold"
                ),
                fill=TEXT
            )


    # ========================================================
    # PARTICULES
    # ========================================================

    def draw_particles(self):

        for particle in self.particles:

            size = max(
                1,
                particle["life"] * 7
            )

            self.canvas.create_oval(
                particle["x"] - size,
                particle["y"] - size,
                particle["x"] + size,
                particle["y"] + size,
                fill=particle["color"],
                outline=""
            )


    # ========================================================
    # EXPLOSION NUCLÉAIRE
    # ========================================================

    def nuke_draw(self):

        progress = (
            1 -
            self.nuke_timer /
            2.5
        )

        radius = (
            40 +
            progress *
            420
        )

        self.canvas.create_oval(
            WIDTH // 2 - radius,
            HEIGHT // 2 - radius,
            WIDTH // 2 + radius,
            HEIGHT // 2 + radius,
            outline=YELLOW,
            width=max(
                2,
                int(
                    8 *
                    (1 - progress)
                    +
                    1
                )
            )
        )

        self.canvas.create_text(
            WIDTH // 2,
            130,
            text="EXPLOSION",
            font=(
                "Arial",
                24,
                "bold"
            ),
            fill=YELLOW
        )


    # ========================================================
    # ÉCLAIR DE TRANSFORMATION
    # ========================================================

    def lightning_draw(self):

        if self.boss not in self.enemies:

            return

        x = self.boss["x"]
        y = self.boss["y"]

        for _ in range(8):

            points = [
                x,
                10
            ]

            current_x = x

            for _ in range(5):

                current_x += random.randint(
                    -45,
                    45
                )

                points.extend([
                    current_x,
                    y -
                    50 +
                    random.randint(
                        -20,
                        20
                    )
                ])

            points.extend([
                x,
                y
            ])

            self.canvas.create_line(
                *points,
                fill=TEXT,
                width=2
            )


    # ========================================================
    # ÉCRAN COMPÉTENCE
    # ========================================================

    def upgrade_draw(self):

        self.canvas.create_rectangle(
            0,
            75,
            WIDTH,
            HEIGHT,
            fill="#020617",
            stipple="gray50",
            outline=""
        )

        self.canvas.create_text(
            WIDTH // 2,
            120,
            text=(
                f"VAGUE "
                f"{self.wave} "
                f"TERMINÉE"
            ),
            font=(
                "Arial",
                30,
                "bold"
            ),
            fill=TEXT
        )

        self.canvas.create_text(
            WIDTH // 2,
            155,
            text="CHOISIS UNE COMPÉTENCE",
            font=(
                "Arial",
                12,
                "bold"
            ),
            fill=MUTED
        )

        for i, upgrade in enumerate(
            self.choices
        ):

            (
                name,
                description,
                rarity,
                skill_class,
                effect,
                value
            ) = upgrade

            x1 = (
                120 +
                i * 260
            )

            x2 = (
                x1 +
                220
            )

            color = (
                RARITY_COLORS[
                    rarity
                ]
            )

            self.canvas.create_rectangle(
                x1,
                245,
                x2,
                480,
                fill="#111827",
                outline=color,
                width=2
            )

            self.canvas.create_text(
                (x1 + x2) // 2,
                280,
                text=rarity.upper(),
                font=(
                    "Arial",
                    9,
                    "bold"
                ),
                fill=color
            )

            self.canvas.create_text(
                (x1 + x2) // 2,
                320,
                text=name,
                font=(
                    "Arial",
                    14,
                    "bold"
                ),
                fill=color
            )

            self.canvas.create_text(
                (x1 + x2) // 2,
                365,
                text=f"[{skill_class}]",
                font=(
                    "Arial",
                    10,
                    "bold"
                ),
                fill=TEXT
            )

            self.canvas.create_text(
                (x1 + x2) // 2,
                410,
                text=description,
                font=(
                    "Arial",
                    10
                ),
                fill=TEXT,
                width=185
            )

            self.canvas.create_text(
                (x1 + x2) // 2,
                450,
                text="CLIQUER",
                font=(
                    "Arial",
                    9,
                    "bold"
                ),
                fill=MUTED
            )


    # ========================================================
    # BOUTIQUE
    # ========================================================

    def shop_draw(self):

        self.canvas.create_rectangle(
            0,
            75,
            WIDTH,
            HEIGHT,
            fill="#020617",
            stipple="gray50",
            outline=""
        )

        self.canvas.create_text(
            WIDTH // 2,
            125,
            text="BOUTIQUE",
            font=(
                "Arial",
                30,
                "bold"
            ),
            fill=TEXT
        )

        self.canvas.create_text(
            WIDTH // 2,
            160,
            text=(
                f"PIÈCES : {self.coins}"
                "   •   "
                "B pour ouvrir"
                "   •   "
                "ÉCHAP pour fermer"
            ),
            font=(
                "Arial",
                11
            ),
            fill=MUTED
        )

        cards = [

            (
                120,
                380,
                "MÉDECIN",
                "+40 PV",
                "20 pièces",
                GREEN
            ),

            (
                400,
                600,
                "BOUCLIER",
                "+1 bouclier",
                "30 pièces",
                CYAN
            ),

            (
                620,
                880,
                "SURPUISSANCE",
                "x2 dégâts pendant 60 s",
                "50 pièces",
                YELLOW
            )
        ]

        for (
            x1,
            x2,
            name,
            description,
            price,
            color
        ) in cards:

            self.canvas.create_rectangle(
                x1,
                190,
                x2,
                290,
                fill="#111827",
                outline=color,
                width=2
            )

            self.canvas.create_text(
                (x1 + x2) // 2,
                220,
                text=name,
                font=(
                    "Arial",
                    15,
                    "bold"
                ),
                fill=color
            )

            self.canvas.create_text(
                (x1 + x2) // 2,
                250,
                text=description,
                font=(
                    "Arial",
                    10
                ),
                fill=TEXT
            )

            self.canvas.create_text(
                (x1 + x2) // 2,
                275,
                text=price,
                font=(
                    "Arial",
                    9,
                    "bold"
                ),
                fill=MUTED
            )

        self.canvas.create_text(
            WIDTH // 2,
            360,
            text=(
                "La boutique met la partie "
                "complètement en pause."
            ),
            font=(
                "Arial",
                11,
                "bold"
            ),
            fill=MUTED
        )

        self.button(
            360,
            500,
            640,
            555,
            "RETOURNER AU JEU",
            PLAYER_COLOR,
            10
        )


    # ========================================================
    # ENCYCLOPÉDIE
    # ========================================================

    def encyclopedia(self):

        self.canvas.create_text(
            WIDTH // 2,
            52,
            text="COMPÉTENCES / MONSTRES",
            font=(
                "Arial",
                30,
                "bold"
            ),
            fill=TEXT
        )

        self.canvas.create_text(
            WIDTH // 2,
            84,
            text=(
                f"{len(UPGRADES)} compétences • "
                "7 raretés • "
                "8 classes • "
                "50 monstres + 2 boss spéciaux"
            ),
            font=(
                "Arial",
                11
            ),
            fill=MUTED
        )

        top = (
            115 -
            self.enc_scroll
        )

        # COMPÉTENCES

        self.canvas.create_text(
            25,
            top,
            text="COMPÉTENCES",
            anchor="w",
            font=(
                "Arial",
                16,
                "bold"
            ),
            fill=CYAN
        )

        y = top + 35

        for upgrade in UPGRADES:

            (
                name,
                description,
                rarity,
                skill_class,
                effect,
                value
            ) = upgrade

            if (
                -30 <
                y <
                HEIGHT + 30
            ):

                color = (
                    RARITY_COLORS[
                        rarity
                    ]
                )

                self.canvas.create_text(
                    28,
                    y,
                    text=f"• {name}",
                    anchor="w",
                    font=(
                        "Arial",
                        10,
                        "bold"
                    ),
                    fill=color
                )

                self.canvas.create_text(
                    210,
                    y,
                    text=f"[{rarity}]",
                    anchor="w",
                    font=(
                        "Arial",
                        9,
                        "bold"
                    ),
                    fill=color
                )

                self.canvas.create_text(
                    310,
                    y,
                    text=f"[{skill_class}]",
                    anchor="w",
                    font=(
                        "Arial",
                        9,
                        "bold"
                    ),
                    fill=TEXT
                )

                self.canvas.create_text(
                    405,
                    y,
                    text=description,
                    anchor="w",
                    font=(
                        "Arial",
                        9
                    ),
                    fill=MUTED
                )

            y += 34

        # MONSTRES

        monster_top = (
            top +
            35
        )

        start_x = 570

        self.canvas.create_text(
            start_x,
            monster_top - 35,
            text="50 MONSTRES + BOSS",
            anchor="w",
            font=(
                "Arial",
                16,
                "bold"
            ),
            fill=RED
        )

        for i, monster in enumerate(
            MONSTERS
        ):

            yy = (
                monster_top +
                i * 44
            )

            if (
                -30 <
                yy <
                HEIGHT - 40
            ):

                (
                    name,
                    min_wave,
                    hp,
                    speed,
                    radius,
                    color,
                    contact,
                    special
                ) = monster

                self.canvas.create_text(
                    start_x,
                    yy,
                    text=(
                        f"• {i + 1}. "
                        f"{name}"
                    ),
                    anchor="w",
                    font=(
                        "Arial",
                        10,
                        "bold"
                    ),
                    fill=color
                )

                self.canvas.create_text(
                    start_x + 190,
                    yy,
                    text=(
                        f"Vague "
                        f"{min_wave}+"
                    ),
                    anchor="w",
                    font=(
                        "Arial",
                        9
                    ),
                    fill=TEXT
                )

                self.canvas.create_text(
                    start_x + 275,
                    yy,
                    text=(
                        f"PV "
                        f"{hp:g}x"
                    ),
                    anchor="w",
                    font=(
                        "Arial",
                        9
                    ),
                    fill=MUTED
                )

        # BOSS SPÉCIAUX

        special_y = (
            monster_top +
            len(MONSTERS) *
            44
        )

        special_bosses = (

            (
                "51. KOKUSHIBO",
                100,
                "Six yeux • lames lunaires • téléportation • régénération",
                RED
            ),

            (
                "BOSS SPÉCIAL : EREN",
                120,
                "Titan assaillant • éclair • explosion • Titan colossal",
                ORANGE
            )
        )

        for (
            label,
            wave,
            description,
            color
        ) in special_bosses:

            if (
                -30 <
                special_y <
                HEIGHT - 40
            ):

                self.canvas.create_text(
                    start_x,
                    special_y,
                    text=f"• {label}",
                    anchor="w",
                    font=(
                        "Arial",
                        10,
                        "bold"
                    ),
                    fill=color
                )

                self.canvas.create_text(
                    start_x + 190,
                    special_y,
                    text=f"Vague {wave}",
                    anchor="w",
                    font=(
                        "Arial",
                        9
                    ),
                    fill=TEXT
                )

                self.canvas.create_text(
                    start_x + 275,
                    special_y,
                    text=description,
                    anchor="w",
                    font=(
                        "Arial",
                        8
                    ),
                    fill=MUTED
                )

            special_y += 44

        self.button(
            WIDTH // 2 - 140,
            590,
            WIDTH // 2 + 140,
            630,
            "ÉCHAP : RETOUR",
            PLAYER_COLOR,
            10
        )

        self.canvas.create_text(
            WIDTH - 20,
            90,
            text="MOLETTE : faire défiler",
            anchor="e",
            font=(
                "Arial",
                9
            ),
            fill=MUTED
        )


    # ========================================================
    # GAME OVER
    # ========================================================

    def gameover_draw(self):

        self.canvas.create_rectangle(
            0,
            75,
            WIDTH,
            HEIGHT,
            fill="#020617",
            stipple="gray50",
            outline=""
        )

        self.canvas.create_text(
            WIDTH // 2,
            210,
            text="GAME OVER",
            font=(
                "Arial",
                48,
                "bold"
            ),
            fill=RED
        )

        self.canvas.create_text(
            WIDTH // 2,
            275,
            text=(
                f"SCORE : "
                f"{self.score}"
            ),
            font=(
                "Arial",
                20,
                "bold"
            ),
            fill=TEXT
        )

        self.canvas.create_text(
            WIDTH // 2,
            310,
            text=(
                f"VAGUE : "
                f"{self.wave}"
            ),
            font=(
                "Arial",
                12
            ),
            fill=MUTED
        )

        self.canvas.create_text(
            WIDTH // 2,
            340,
            text=(
                f"MEILLEUR SCORE : "
                f"{self.high_score}"
            ),
            font=(
                "Arial",
                12
            ),
            fill=MUTED
        )

        self.button(
            WIDTH // 2 - 160,
            385,
            WIDTH // 2 + 160,
            445,
            "ENTRÉE POUR REJOUER",
            PLAYER_COLOR,
            13
        )


    # ========================================================
    # FIN
    # ========================================================

    def end_game(self):

        self.running = False

        self.game_over = True

        self.mouse_down = False

        self.high_score = max(
            self.high_score,
            self.score
        )


# ============================================================
# LANCEMENT
# ============================================================

if __name__ == "__main__":

    root = tk.Tk()

    game = NeonArena(
        root
    )

    root.mainloop()
