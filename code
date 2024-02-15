# eggcatcher
from itertools import cycle
from random import randrange
from tkinter import Canvas, Tk, messagebox, font


messagebox.showinfo("Welcome", "Welcome to Ball Drop!")
messagebox.showinfo("Instructions", "Don't let the balls hit the ground. Look out for golden balls, they give you extra lives. Good luck!")


#creating canvas
canvas_width = 800
canvas_height = 400

root = Tk()
root.title("Ball Drop")


c = Canvas(root, width=canvas_width, height=canvas_height, background="light pink")
c.create_rectangle(-5, canvas_height-100, canvas_width+5, canvas_height+5, fill="paleturquoise", width=0)
c.create_oval(-80, -80, 120, 120, fill='yellow', width=0)
c.pack()


#creating ball
color_cycle = cycle(["light blue", "light green", "aqua", "light yellow", "light cyan"])
ball_width = 55
ball_height = 55
ball_score = 10
ball_speed = 300
ball_interval = 3000
difficulty = 0.95

#create magic ball
magicball_width = 55
magicball_height = 55
magicball_speed = 300
magicball_interval = 10000

#creating basket
basket_color = "hotpink"
basket_width = 100
basket_height = 100
basket_startx = canvas_width / 2 - basket_width / 2
basket_starty = canvas_height - basket_height - 20

basket_startx2 = basket_startx + basket_width
basket_starty2 = basket_starty + basket_height

##tool to create an arc
basket = c.create_arc(basket_startx, basket_starty, basket_startx2, basket_starty2, start=200, extent=140, style="arc", outline=basket_color, width=3)


#create score and lives
game_font = font.nametofont("TkFixedFont")
game_font.config(size=18)

score=0
score_text= c.create_text(100,35,activefill="light pink", anchor="ne", font=game_font, fill="darkblue", text="Score: " + str(score))

lives_remaining = 3
lives_text = c.create_text(canvas_width-10, 35, anchor="ne", font=game_font, fill="yellow", text="Lives: "+ str(lives_remaining))


##game

balls = []
magicballs=[]

def create_ball():
    x = randrange(20, 760)
    y = 45
    new_ball=c.create_oval(x, y, x+ball_width, y+ball_height, fill=next(color_cycle), width=0)
    ###adds new ball to the list
    balls.append(new_ball)
      ### repeats the function
    root.after(ball_interval, create_ball)

def create_magicball ():
    xx = randrange(20, 780)
    yy = 45
    new_magicball=c.create_oval(xx, yy, xx+magicball_width, yy+magicball_height, fill="gold", outline="white",width=4)
    magicballs.append(new_magicball)
    root.after(magicball_interval, create_magicball)

    
def move_ball():
    for ball in balls:
        (ballx, bally, ballx2, bally2) = c.coords(ball)
        c.move(ball, 0, 10)
        if bally2 > canvas_height:
            ball_ground(ball)
    root.after(ball_speed, move_ball)

def move_magicball():
    for magicball in magicballs:
        (magicballx, magicbally, magicballx2, magicbally2) = c.coords(magicball)
        c.move(magicball, 0, 10)
        if magicbally2 > canvas_height:
            magicball_ground(magicball)
    root.after(magicball_speed, move_magicball)

def ball_ground(ball):
    balls.remove(ball)
    c.delete(ball)
    lose_a_life()
    if lives_remaining == 0:
        messagebox.showinfo("Game Over!", "Final Score: "+ str(score))
        root.destroy()

def magicball_ground(magicball):
    magicballs.remove(magicball)
    c.delete(magicball)  

def lose_a_life():
    global lives_remaining
    lives_remaining -= 1
    c.itemconfigure(lives_text, text="Lives: "+ str(lives_remaining))

def increase_life():
    global lives_remaining
    lives_remaining += 1
    c.itemconfigure(lives_text, text="Lives: "+ str(lives_remaining))

def catch_ball():
    (basketx, baskety, basketx2, baskety2) = c.coords(basket)
    for ball in balls:
        (ballx, bally, ballx2, bally2) = c.coords(ball)
        if basketx < ballx and ballx2 < basketx2 and baskety2 - bally2 < 40:
            balls.remove(ball)
            c.delete(ball)
            increase_score(ball_score)
    root.after(100,catch_ball)
        

def catch_magicball():
    (basketx, baskety, basketx2, baskety2) = c.coords(basket)
    for magicball in magicballs:
        (magicballx, magicbally, magicballx2, magicbally2) = c.coords(magicball)
        if basketx < magicballx and magicballx2 < basketx2 and baskety2 - magicbally2 < 40:
            magicballs.remove(magicball)
            c.delete(magicball)
            increase_life()
    root.after(100,catch_magicball)

def increase_score(points):
    global score, ball_speed, ball_interval
    score += points
    ball_speed = int(ball_speed * difficulty)
    ball_interval = int(ball_interval * difficulty)
    c.itemconfigure(score_text, text="Score:"+ str(score))

def move_left(event):
    (x1, y1, x2, y2) = c.coords(basket)
    if x1 > 0:
        c.move(basket, -20, 0)

def move_right(event):
    (x1, y1, x2, y2) = c.coords(basket)
    if x2 < canvas_width:
        c.move(basket, 20, 0)


c.bind("<Left>", move_left)
c.bind("<Right>", move_right)
c.focus_set()
root.after(1000, create_ball)
root.after(1000, move_ball)
root.after(1000, catch_ball)
root.after(2000, create_magicball)
root.after(1000, move_magicball)
root.after(1000, catch_magicball)
root.mainloop()
   
