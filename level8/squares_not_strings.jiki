//checks if the emoji is ok to pick up
function is_valid_emoji with emoji do
  set non_valid_emojis to ["⭐", "🏁", "⬜", ""]
  for each item in non_valid_emojis do
    if item == emoji do
      return false
    end
  end
  return true
end

function turn_around do
  turn_right()
  turn_right()
end

function is_square_available with direction do
  set result to look(direction)
  if !result.in_maze or result.is_wall or result.contents == "🔥" or result.contents == "💩" do
    return false
  end
  return true
end

function can_turn_left do
  return is_square_available("left")
end

function can_move do
  return is_square_available("ahead")
end

function can_turn_right do
  return is_square_available("right")
end

set emoji_list to {}
set emoji to ""

repeat_until_game_over do
  if can_turn_left() do
    turn_left()
  else if can_move() do
  else if can_turn_right() do
    turn_right()
  else do
    turn_around()
  end
  move()

  change emoji to look("down").contents

  if is_valid_emoji(emoji) do
    if has_key(emoji_list, emoji) do
      change emoji_list[emoji] to emoji_list[emoji] + 1
    else do 
      change emoji_list[emoji] to 1
    end
    remove_emoji()
  else if look("down").is_finish do
    announce_emojis(emoji_list)
  end
  
end