//shamefully solved with someone elses ideas
set sky to new Sky(0)
set left_window to new Window(30, 55, 12, 13, 6)
set right_window to new Window(58, 55, 12, 13, 6)

set sun to new Sun(80, 20, 10, 1)
set parts_list to [sky, new Ground(20, 2), new Frame(20, 50, 60, 40, 6), new Roof(50, 30, 68, 20, 6), new Door(43,72, 14, 18, 6)]
repeat 70 times do
  change sun.cx to sun.cx - 1.2
  change sun.cy to sun.cy + 1
end

set bright to 100
change left_window.lights to true
change right_window.lights to true
repeat 80 times do
  change bright to bright - 1
  for each part in parts_list do
    change part.brightness to bright
  end
  change sky.hue to min((sky.hue + 2), 310)
end