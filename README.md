import tkinter as tk
from tkinter import messagebox
import random

# Player data
players = [
    {"name": "Rohit Sharma", "role": "Batsman", "price": 90},
    {"name": "David Warner", "role": "Batsman", "price": 85},
    {"name": "Shubman Gill", "role": "Batsman", "price": 80},
    {"name": "Steve Smith", "role": "Batsman", "price": 75},
    {"name": "KL Rahul", "role": "Batsman", "price": 85},
    {"name": "Jasprit Bumrah", "role": "Bowler", "price": 95},
    {"name": "Rashid Khan", "role": "Bowler", "price": 90},
    {"name": "Hardik Pandya", "role": "All-Rounder", "price": 90},
    {"name": "Ben Stokes", "role": "All-Rounder", "price": 95},
    {"name": "Sanju Samson", "role": "Wicketkeeper", "price": 80},
]

# Global variables
csk_budget = 1000
rcb_budget = 1000
csk_team = []
rcb_team = []
current_player = None
current_bid = 0

# Functions
def start_auction():
    global current_player, current_bid
    if not players:
        messagebox.showinfo("Auction Over", "All players have been auctioned!")
        show_teams()
        return

    current_player = random.choice(players)
    players.remove(current_player)
    current_bid = current_player["price"]
    update_info(f"Player: {current_player['name']} ({current_player['role']}) - Base Price: ₹{current_bid} lakh")
    enable_buttons()

def csk_bid():
    global csk_budget, current_bid
    if csk_budget < current_bid + 1:
        messagebox.showinfo("Budget Exceeded", "CSK doesn't have enough budget!")
        return
    current_bid += 1
    csk_budget -= 1
    update_info(f"CSK bid ₹{current_bid} lakh for {current_player['name']}")

def rcb_bid():
    global rcb_budget, current_bid
    if rcb_budget < current_bid + 1:
        messagebox.showinfo("Budget Exceeded", "RCB doesn't have enough budget!")
        return
    current_bid += 1
    rcb_budget -= 1
    update_info(f"RCB bid ₹{current_bid} lakh for {current_player['name']}")

def csk_pass():
    finalize_bid("RCB")

def rcb_pass():
    finalize_bid("CSK")

def finalize_bid(winner):
    global csk_budget, rcb_budget, csk_team, rcb_team
    if winner == "CSK":
        csk_team.append(current_player)
        messagebox.showinfo("Player Sold", f"{current_player['name']} sold to CSK for ₹{current_bid} lakh!")
    elif winner == "RCB":
        rcb_team.append(current_player)
        messagebox.showinfo("Player Sold", f"{current_player['name']} sold to RCB for ₹{current_bid} lakh!")
    disable_buttons()
    start_auction()

def update_info(text):
    info_label.config(text=text)

def show_teams():
    csk_team_info = "\n".join([f"{player['name']} - ₹{player['price']} lakh" for player in csk_team])
    rcb_team_info = "\n".join([f"{player['name']} - ₹{player['price']} lakh" for player in rcb_team])
    messagebox.showinfo("CSK Team", f"Remaining Budget: ₹{csk_budget} lakh\n\n{csk_team_info}")
    messagebox.showinfo("RCB Team", f"Remaining Budget: ₹{rcb_budget} lakh\n\n{rcb_team_info}")

def disable_buttons():
    csk_bid_button.config(state=tk.DISABLED)
    rcb_bid_button.config(state=tk.DISABLED)
    csk_pass_button.config(state=tk.DISABLED)
    rcb_pass_button.config(state=tk.DISABLED)

def enable_buttons():
    csk_bid_button.config(state=tk.NORMAL)
    rcb_bid_button.config(state=tk.NORMAL)
    csk_pass_button.config(state=tk.NORMAL)
    rcb_pass_button.config(state=tk.NORMAL)

# GUI Setup
window = tk.Tk()
window.title("IPL Auction Game")

info_label = tk.Label(window, text="Press 'Start Auction' to begin", font=("Arial", 14))
info_label.pack(pady=10)

start_button = tk.Button(window, text="Start Auction", font=("Arial", 12), command=start_auction)
start_button.pack(pady=5)

csk_bid_button = tk.Button(window, text="CSK Bid", font=("Arial", 12), command=csk_bid, state=tk.DISABLED)
csk_bid_button.pack(pady=5)

rcb_bid_button = tk.Button(window, text="RCB Bid", font=("Arial", 12), command=rcb_bid, state=tk.DISABLED)
rcb_bid_button.pack(pady=5)

csk_pass_button = tk.Button(window, text="CSK Pass", font=("Arial", 12), command=csk_pass, state=tk.DISABLED)
csk_pass_button.pack(pady=5)

rcb_pass_button = tk.Button(window, text="RCB Pass", font=("Arial", 12), command=rcb_pass, state=tk.DISABLED)
rcb_pass_button.pack(pady=5)

window.mainloop()
