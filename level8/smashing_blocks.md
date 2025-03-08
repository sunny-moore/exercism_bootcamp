// Create an instance of a Ball and move it around
set ball to new Ball()
set right_bottom to 100 - ball.radius
set left_top to ball.radius
set blocks to [new Block(8, 31), new Block(25, 31), new Block(42, 31), new Block(59, 31), new Block(76, 31)]
set smashed_list to []

function check_at_wall with a_ball, right_bottom, left_top do
  if a_ball.cx == right_bottom do
    change a_ball.x_velocity to - 1
  else if a_ball.cx == left_top do
    change a_ball.x_velocity to 1
  end
  if a_ball.cy == right_bottom do
    change a_ball.y_velocity to - 1
  else if a_ball.cy == left_top do
    change a_ball.y_velocity to 1
  end
end

function change_dir with a_ball do
  change a_ball.y_velocity to a_ball.y_velocity * -1
end

function check_at_block with a_ball, block_list do
  set smashed to []
    for each block in block_list do
      if !block.smashed and (a_ball.cx >= block.left and a_ball.cx <= (block.left + block.width)) and ((a_ball.cy == (block.top + block.height + a_ball.radius)) or  (a_ball.cy == (block.top - a_ball.radius)))  do
        change block.smashed to true
        change_dir(a_ball)
      end
      if block.smashed do
        change smashed to push(smashed, block)
      end
    end
  return smashed
end

repeat_until_game_over do
  change smashed_list to check_at_block(ball, blocks)
  check_at_wall(ball, right_bottom, left_top)
  if my#length(smashed_list) < 5 do
    move_ball(ball)
  else do
    break
  end
end