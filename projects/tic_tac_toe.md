function run_game with moves do
  log moves
  set hex_color to "#000000"
  set board to [["", "", ""], ["", "", ""], ["", "", ""]]
  set total_moves to length(moves)
  set result to []
  set player to ""

  draw_board()
  
  for each item in moves indexed by i do
    if i % 2 == 1 do
      change player to "o"
    else do
      change player to "x"
    end
  
    if item == "?" do
      change item to take_turn([item, board, player, i, total_moves])
      //strickly so that it will draw a "draw" correctly.
      change moves[i] to item 
    end
    if is_valid(board, item) do
      if player == "o" do 
        draw_square(item, player, hex_color)
        change board[item[1]][item[2]] to "o"
        change result to get_result(item, board, player, i, total_moves)
      else do
        draw_square(item, player, hex_color)
        change board[item[1]][item[2]] to "x"
        change result to get_result(item, board, player, i, total_moves)
      end  
    else do
      change result[1] to "invalid"
      draw_invalid()
      break 
    end
  end
  
  //if result is a win
  if result[1] == "win" do
    draw_win(moves, result)
  end
  //game was a draw
  if result[1] == "draw" do
    draw_draw(moves)
  end
end

//The AI turn, current_state is [item, board, player, turn_number, total_turns]
function take_turn with current_state do
  log(current_state)
  set turn to check_for_win_or_draw_turn(current_state)
  log(turn)
  if turn[1] == "win" or turn[1] == "draw" do
    return turn[2]
  end
  change turn to check_blocking_turn(current_state)
  if turn[1] == "win" do
    return turn[2]
  end
  change turn to check_next_turn(current_state)
  return turn[2]
end

//current_state is [coords, board, player, turn_number, total_moves]
function check_for_win_or_draw_turn with current_state do
  set current_board to current_state[2]
  set future_board to current_board
  set next_move to []
  set result to 0
  
  for each row in current_board indexed by i do
    for each col in row indexed by j do
      if col == "" do
        change next_move to[i, j]
        log(next_move)
        log(future_board)
        change future_board[i][j] to current_state[3]
        change result to get_result(next_move, future_board, current_state[3], current_state[4], current_state[5])
        if result[1] == "win" or result[1] == "draw" do
          return result
        else do
          change future_board[i][j] to ""
        end
      end
    end
  end
  return result
end

//current_state is [coords, board, player, turn_number, total_moves]
function check_blocking_turn with current_state do
  set player_to_block to switch_player(current_state[4])
  set current_board to current_state[2]
  set future_board to current_board
  set next_move to []
  set result to 0
  
  for each row in current_board indexed by i do
    for each col in row indexed by j do
      if col == "" do
        change next_move to[i, j]
        log(next_move)
        log(future_board)
        change future_board[i][j] to player_to_block
        change result to get_result(next_move, future_board, player_to_block, current_state[4], current_state[5])
        if result[1] == "win" or result[1] == "draw" do
          return result
        else do
          change future_board[i][j] to ""
        end
      end
    end
  end
  //reached if there are opponent win moves to be made at current state.
  //returns last result
  return result
end

//takes the next available turn
//current_state is [coords, board, player, turn_number, total_moves]
function check_next_turn with current_state do
  for each row in current_state[2] indexed by i do
    for each col in row indexed by j do
      if col == "" do
        return ["go", [i, j], current_state[2], current_state[3], current_state[4]]
      end
    end
  end
end
//used by check_blocking_turn to see if opposite player can win in next move
function switch_player with turn_number do
  if turn_number % 2 == 1 do
    return "x"
  else do
    return "o"
  end
end

//tells if current move is a win, draw or neither (go)
function get_result with item, board, player, turn_number, total_moves do
  set result to []

  if board[1][1] == player and board[1][2] == player and board[1][3] == player  do
    change result to ["win", item, player, [[1,1], [1,2], [1,3]]]
  else if board[2][1] == player and board[2][2] == player and board[2][3] == player  do
    change result to ["win", item, player, [[2,1],[2,2],[2,3]]]
  else if board[3][1] == player and board[3][2] == player and board[3][3] == player  do
    change result to ["win", item, player, [[3,1],[3,2],[3,3]]]
  else if board[1][1] == player and board[2][1] == player and board[3][1] == player  do
    change result to ["win", item, player, [[1,1],[2,1],[3,1]]]
  else if board[1][2] == player and board[2][2] == player and board[3][2] == player  do
    change result to ["win", item, player, [[1,2],[2,2],[3,2]]]
  else if board[1][3] == player and board[2][3] == player and board[3][3] == player  do
    change result to ["win", item, player, [[1,3],[2,3],[3,3]]]
  else if board[1][1] == player and board[2][2] == player and board[3][3] == player  do
    change result to ["win", item, player, [[1,1],[2,2],[3,3]]]
  else if board[1][3] == player and board[2][2] == player and board[3][1] == player  do
    change result to ["win", item, player, [[1,3],[2,2],[3,1]]]
  //if we aren't at a win, check for a draw
  else if turn_number == 9 do
    change result to ["draw", item, player, []]
  //if neither of these is true, we go on to next turn
  else do
    change result to ["go", item, player, []]
  end
  return result
end

function is_valid with board, coords do
  if board[coords[1]][coords[2]] == "" do
    return true
  end
  return false
end

function length with string do
  set count to 0
  for each char in string do
    change count to count + 1
  end
  return count
end

//result is ["win"/"draw", coords, player, list of winning moves]
function draw_win with moves, result do
  set win_hex_color to "#604fcd"
  set hex_color to "#D3D3D3" //light grey
  set wins to result[4]
  //draw winning moves 
  for each item in wins do
    if result[3] == "o" do
      draw_square(item, "o", win_hex_color)
    else if result[3] == "x" do
      draw_square(item, "x", win_hex_color)
    end
  end

  //draw non-winning moves
  for each item in moves indexed by i do 
  //draw everything BUT the winning pieces
    if (item[1] == wins[1][1] and item[2] == wins[1][2]) or (item[1] == wins[2][1] and item[2] == wins[2][2]) or (item[1] == wins[3][1] and item[2] == wins[3][2]) do
      next
    else do
      if i % 2 == 1 do
        draw_square(item, "o", hex_color)
      else do
        draw_square(item, "x", hex_color)
      end
    end
  end
                                                                                                                                          
  fill_color_rgba(96, 79, 205, 0.85)
  rectangle(0, 0, 100, 100)
  write(concatenate("The ", result[3], "'s won!"))
end

function draw_invalid do
  fill_color_rgba(200, 0, 0, 0.85)
  rectangle(0, 0, 100, 100)
  write("Invalid move!")                                                                                                                                       
end

function draw_draw with moves do                                                                                                                                        
  set hex_color to "#D3D3D3"

  for each item in moves indexed by i do 
    if i % 2 == 1 do
      draw_square(item, "o", hex_color)
    else do
      //This is rgba for hex #D3D3D3
      draw_square(item, "x", hex_color)
    end
  end
  fill_color_rgba(96, 79, 205, 0.85)
  rectangle(0, 0, 100, 100)
  write("The game was a draw!") 
end

function draw_board do
  draw_rectangle()
  draw_board_lines()
end

//The board outline
function draw_rectangle do
  rectangle(5, 5, 90, 90)
  fill_color_rgba(254, 254, 254, 0)
  stroke_width(1)
  stroke_color_hex("#000000")
end

//The grid lines
function draw_board_lines do
  set offset to 30
  fill_color_rgba(0, 0, 0, 1)
  line(35, 5, 35, 95)
  line(65, 5, 65, 95)
  line(5, 35, 95, 35)
  line(5, 65, 95, 65)
end

function draw_o with cx_cy_list, hex_color do
  set radius to 10
  stroke_width(1)
  stroke_color_hex(hex_color)
  fill_color_rgba(254, 254, 254, 0)
  circle(cx_cy_list[1], cx_cy_list[2], radius)
end

function draw_x with cx_cy_list, hex_color do
  stroke_width(1)
  stroke_color_hex(hex_color)
  line(cx_cy_list[1] - 10, cx_cy_list[2] - 10, cx_cy_list[1] + 10, cx_cy_list[2] + 10)
  line(cx_cy_list[1] - 10, cx_cy_list[2] + 10, cx_cy_list[1] + 10, cx_cy_list[2] - 10)
end

function draw_square with coords, player, hex_color do
  set squares to [[1, 1], [1, 2], [1,3], [2, 1], [2, 2], [2,3], [3, 1], [3, 2], [3,3]]
  set square_centers to [[20, 20], [50, 20], [80, 20],[20, 50], [50, 50], [80, 50], [20, 80], [50, 80], [80, 80]] 

  for each pair in squares indexed by i do
      if pair[1] == coords[1] and pair[2] == coords[2] do
        if player == "o" do
          draw_o(square_centers[i], hex_color)
        else do
          draw_x(square_centers[i], hex_color)
        end
      end
  end
end