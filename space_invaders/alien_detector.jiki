//function takes the row of aliens and checks if they are all dead
function aliens_dead with aliens_in_row do
  for each alien in aliens_in_row do
    if alien do
        return false
    end
  end
  return true
end

// Get the first row of aliens (the bottom one)
set row to 1
set aliens_in_row to get_starting_aliens_in_row(row)

// Set variables about facts
set left_boundary to 1
set right_boundary to 11

// Set variables to track things
set direction to "right"
set position to 1

// Play the game
repeat_until_game_over do
  
  if aliens_in_row[position] do
    shoot()
    change aliens_in_row[position] to false //we killed the alien
  end
 
// Move along
  if direction is "right" do
    move_right()
    change position to position + 1
  else if direction is "left" do
    move_left()
    change position to position - 1
  end

  // If we hit an edge, change direction
  if position >= right_boundary do
    change direction to "left"
  else if position <= left_boundary do
    change direction to "right"
  end

  //check if we killed the aliens in this row then get new row
    if row == 1 and aliens_dead(aliens_in_row) do
      change row to row + 1
      change aliens_in_row to get_starting_aliens_in_row(row)
    end
    if row == 2 and aliens_dead(aliens_in_row) do
        change row to row + 1
        change aliens_in_row to get_starting_aliens_in_row(row)
    end
    if row == 3 and aliens_dead(aliens_in_row) do
      fire_fireworks()
    end
end
