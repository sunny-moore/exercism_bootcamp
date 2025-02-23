// Receives a number as its input
// Should return the number of steps to reach 1
function collatz_steps with number do
  set step_count to 0
  set current_number to number

  repeat_forever do
    if current_number == 1 do
      return step_count
    else if current_number % 2 == 0 do
      change current_number to current_number / 2

    else if current_number % 2 == 1 do
      change current_number to current_number * 3 + 1
      
    end
    change step_count to step_count + 1
  end
end