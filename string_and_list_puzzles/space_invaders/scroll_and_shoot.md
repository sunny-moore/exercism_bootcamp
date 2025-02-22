// TODO: Set any initial variables here
set direction to "right"
set number_of_moves to 10

repeat_until_game_over do
 //keep going right 10 times
  if direction == "right" and number_of_moves > 0  do
    if is_alien_above() do
      shoot()
      move_right()
      else do
        move_right()
    end
    change number_of_moves to number_of_moves - 1
  //when at the right, change direction
  else if direction == "left" and number_of_moves < 10 do
    if is_alien_above() do
      shoot()
      move_left()
      else do
        move_left()  
    end
    change number_of_moves to number_of_moves + 1
  end
  //know when to change direction
  if number_of_moves == 0 do
    change direction to "left"
  else if number_of_moves == 10 do
    change direction to "right"
  end
end