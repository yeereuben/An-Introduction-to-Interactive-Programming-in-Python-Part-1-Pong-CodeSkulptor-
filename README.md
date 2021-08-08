# An-Introduction-to-Interactive-Programming-in-Python-Part-1-Pong-CodeSkulptor-
This is my implementation of Pong, a classic Arcade game. It's a project assignment, in Rice University's An Introduction to Interactive Programming in Python (Part 1), found on Coursera.

# draw mid line and gutters
canvas.draw_line([WIDTH / 2, 0],[WIDTH / 2, HEIGHT], 1, "White")
canvas.draw_line([PAD_WIDTH, 0],[PAD_WIDTH, HEIGHT], 1, "White")
canvas.draw_line([WIDTH - PAD_WIDTH, 0],[WIDTH - PAD_WIDTH, HEIGHT], 1, "White")
    
# update ball
# ball_pos is updated when draw_handler is called (once every 60 secs)
# First, check if updated ball_pos hits top or bottom, so
# ...check & do sth if (y1): ball_pos[1] + ball_vel[1] <= 0 + Ball_rad(top);
# check & do sth if (y1): ball_pos[1] + ball_vel[1] >= 400 - ball_rad (bot)
 
#check if ball hits top    
if (ball_pos[1] + ball_vel[1] <= 0 + BALL_RADIUS): 
    #print("Ball hit the top")
    #print(ball_pos, ball_vel, 'top')
    #reflect ball vel
    ball_vel[1] = - ball_vel[1]
    #print(ball_vel,'top')
    ball_pos[0] = ball_pos[0] + ball_vel[0]
    ball_pos[1] = BALL_RADIUS + ball_vel[1] - (ball_pos[1] - BALL_RADIUS)
    
#check if ball hits bottom    
elif (ball_pos[1] + ball_vel[1] >= HEIGHT - BALL_RADIUS):
    #print("Ball hit the bottom")
    #print(ball_pos, ball_vel,'bott')
    ball_vel[1] = - ball_vel[1] # vert vel is reflected
    #print(ball_vel, 'bott')
    ball_pos[0] = ball_pos[0] + ball_vel[0] 
    ball_pos[1] = (HEIGHT - BALL_RADIUS) + ball_vel[1] - (ball_pos[1] - (HEIGHT - BALL_RADIUS))
    
#check that ball is not touching top and bottom borders    
elif (ball_pos[1] + ball_vel[1] <=400 - BALL_RADIUS and 
      ball_pos[1] + ball_vel[1] >= 0 + BALL_RADIUS):  
    #check if ball is touching left gutter                                   
    if (ball_pos[0] <= PAD_WIDTH + BALL_RADIUS):
        #while ball touches left gutter, check if it touches paddle1
        if (ball_pos[1] >= paddle1_pos - HALF_PAD_HEIGHT 
            and ball_pos[1] <= paddle1_pos + HALF_PAD_HEIGHT):                
            #check to see if ball is already moving right, away from
            #paddle1, make sure it keeps moving right upon collision
            #so ball will not be stuck in paddle
            if (ball_vel[0] < 0):
                ball_vel[0] = - (ball_vel[0] * 1.1)
                ball_vel[1] = ball_vel[1] * 1.1         
                ball_pos[0] = ball_pos[0] + ball_vel[0]
                ball_pos[1] = ball_pos[1] + ball_vel[1]
            else:                           
                ball_pos[0] = ball_pos[0] + ball_vel[0]
                ball_pos[1] = ball_pos[1] + ball_vel[1]
        
        #if ball is not touching left paddle        
        elif (ball_pos[1] <= paddle1_pos - HALF_PAD_HEIGHT
              or ball_pos[1] >= paddle1_pos + HALF_PAD_HEIGHT):
            #assign win to player2 (right) and start new game 
            win = player2
            new_game()
            
    #check if ball is touching right gutter        
    elif(ball_pos[0] >= WIDTH - PAD_WIDTH - BALL_RADIUS):
        #while ball touches right gutter, check if ball touches paddle
        if (ball_pos[1] >= paddle2_pos - HALF_PAD_HEIGHT
            and ball_pos[1] <= paddle2_pos + HALF_PAD_HEIGHT):
            #check to see if ball is already moving left, away from 
            #paddle2, make sure it keeps moving left upon collision
            #so ball will not be stuck in paddle
            if (ball_vel[0] > 0):
                ball_vel[0] = - (ball_vel[0] * 1.1)
                ball_vel[1] = ball_vel[1] * 1.1
                ball_pos[0] = ball_pos[0] + ball_vel[0]
                ball_pos[1] = ball_pos[1] + ball_vel[1]
            else:                           
                ball_pos[0] = ball_pos[0] + ball_vel[0]
                ball_pos[1] = ball_pos[1] + ball_vel[1]
        #if ball is not touching right paddle        
        elif (ball_pos[1] <= paddle2_pos - HALF_PAD_HEIGHT
              or ball_pos[1] >= paddle2_pos + HALF_PAD_HEIGHT):
            #assign win to player1 (left) and start new game
            win = player1
            new_game()
        
        
    #print("Ball is within bounds")
    #print(ball_pos, ball_vel, 'within')
    ball_pos[0] = ball_pos[0] + ball_vel[0]
    ball_pos[1] = ball_pos[1] + ball_vel[1]
        
# draw ball
canvas.draw_circle(ball_pos, BALL_RADIUS, 1, 'White', 'White')

# update paddle's vertical position, keep paddle on the screen
if (paddle1_pos + paddle1_vel >= HALF_PAD_HEIGHT and
    paddle1_pos + paddle1_vel <= HEIGHT - HALF_PAD_HEIGHT):
    
    paddle1_pos = paddle1_pos + paddle1_vel
elif (paddle1_pos + paddle1_vel < HALF_PAD_HEIGHT or
      paddle1_pos + paddle1_vel > HEIGHT - HALF_PAD_HEIGHT):
    paddle1_pos = paddle1_pos

if (paddle2_pos + paddle2_vel >= HALF_PAD_HEIGHT and
    paddle2_pos + paddle2_vel <= HEIGHT - HALF_PAD_HEIGHT):
    
    paddle2_pos = paddle2_pos + paddle2_vel        
elif (paddle2_pos + paddle2_vel < HALF_PAD_HEIGHT or
      paddle2_pos + paddle2_vel > HEIGHT - HALF_PAD_HEIGHT):
    paddle2_pos = paddle2_pos

    
# draw paddles

canvas.draw_line([HALF_PAD_WIDTH, paddle1_pos - HALF_PAD_HEIGHT],
                 [HALF_PAD_WIDTH, paddle1_pos + HALF_PAD_HEIGHT], 
                 PAD_WIDTH, "White")

canvas.draw_line([WIDTH - HALF_PAD_WIDTH, paddle2_pos - HALF_PAD_HEIGHT],
                 [WIDTH - HALF_PAD_WIDTH, paddle2_pos + HALF_PAD_HEIGHT],
                 PAD_WIDTH, "White")
    
# draw scores and update win variable
canvas.draw_text((str(score1)), (130, 50), 40, 'White')
canvas.draw_text((str(score2)), (460, 50), 40, 'White')

return
