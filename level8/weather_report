// Draw the sky
function draw_sky do
  fill_color_hex("#ADD8E6") 
  rectangle(25, 4, 50, 50)
  rectangle(1, 66, 30, 30)
  rectangle(35, 66, 30, 30)
  rectangle(69, 66, 30, 30)
end

// Draw the sun for sunshine //p = [ x, y, scale]
function draw_sunshine with p do
  fill_color_hex("#ffed06")
  circle(50 * p[3] + p[1], 50 * p[3] + p[2], 25 * p[3])
end

// Sun //p = [ x, y, scale]
function draw_sun with p do
  fill_color_hex("yellow")
  //circle(75, 30, 15)
    circle(75 * p[3] + p[1], 30 * p[3] + p[2], 15 * p[3]) //x is .75 of 50+25, y is .5 + 4
end

// Cloud //p = [ x, y, scale]
function draw_cloud with p do
  fill_color_hex("white")
  rectangle((25 * p[3] + p[1]), (50 * p[3]) + p[2], (50 * p[3]), 10 * p[3])
  //left bottom circle
  circle(25 * p[3] + p[1], 50 * p[3] + p[2], 10 * p[3])
  //right bottom circle
  circle(75 * p[3] + p[1], 50 * p[3] + p[2], 10 * p[3])
  //left top circle
  circle(40 * p[3] + p[1], 40 * p[3] + p[2], 15 * p[3])
  // right top circle
  circle(55 * p[3] + p[1], 40 * p[3] + p[2], 20 * p[3])
end

// Rain Drops //p = [ x, y, scale]
function draw_rain  with p do 
  fill_color_hex("blue")
  ellipse(30 * p[3] + p[1], 70 * p[3] + p[2], 3 * p[3], 5 * p[3])
  ellipse(50 * p[3] + p[1], 70 * p[3] + p[2], 3 * p[3], 5 * p[3])
  ellipse(70 * p[3] + p[1], 70 * p[3] + p[2], 3 * p[3], 5 * p[3])
  ellipse(40 * p[3] + p[1], 80 * p[3] + p[2], 3 * p[3], 5 * p[3])
  ellipse(60 * p[3] + p[1], 80 * p[3] + p[2], 3 * p[3], 5 * p[3])
end

//snow //p = [ x, y, scale]
function draw_snow with p do
  fill_color_hex("white")
  circle(30 * p[3] + p[1], 70 * p[3] + p[2], 5 * p[3])
  circle(50 * p[3] + p[1], 70 * p[3] + p[2], 5 * p[3])
  circle(70 * p[3] + p[1], 70 * p[3] + p[2], 5 * p[3])
  circle(40 * p[3] + p[1], 80 * p[3] + p[2], 5 * p[3])
  circle(60 * p[3] + p[1], 80 * p[3] + p[2], 5 * p[3])
end

function return_elements_list do
  return ["sunny", ["sun"], "dull", ["cloud"], "miserable", ["cloud", "rain"], 
          "hopeful", ["sun", "cloud"], "rainbow-territory", ["sun", "cloud", "rain"], 
          "exciting", ["cloud", "snow"], "snowboarding-time", ["sun", "cloud", "snow"]]
end

//takes the list of dictionaries and adds the args to each item
function get_box_args with time_descript_elements_list do
  set args to [[25, 4, 0.5], [1, 66, 0.3], [35, 66, 0.3], [69, 66, 0.3]]
  
  for each item in time_descript_elements_list indexed by i do
    change item["args"] to args[i]
  end
  return time_descript_elements_list
end

//takes the list of dictionaries, and adds the elements to it
function get_elements with descript_with_time_list do
  set descript_with_elements to return_elements_list()
  set time_descrip_elements to descript_with_time_list

  for each item in descript_with_time_list indexed by i do
    for each item2 in descript_with_elements indexed by i2 do
      if item2 == item["description"] and i2 < 14 do
        log item2
        change time_descrip_elements[i]["elements"] to descript_with_elements[(i2 + 1)]
        next
      end
    end
  end
 return time_descrip_elements
end

//takes a list of dictionaries, and target times 
//returns a list with just those time/description k,v pairs
function get_descriptions_with_time with summary, time do
  set descriptions_with_time to [{}, {}, {}, {}]

  for each k, v in time indexed by i do
    for each item in summary indexed by i2 do
      if item["time"] == v do
        change descriptions_with_time[i]["time"] to item["time"]
        change descriptions_with_time[i]["description"] to item["description"]
      end
    end
  end
  return descriptions_with_time
end

//takes description dictionarytimes we want to display, and date we are looking for
//returns list of dictionaries holding time, description, elements and args
function json_parser with description, time, target_date do
  set summary to description["meteorological"][target_date[1]][target_date[2]][target_date[3]]["weather"]["summary"]
  set descript_with_time to get_descriptions_with_time(summary, time)
  set time_descript_elements to get_elements(descript_with_time)
  set time_descript_elements_args to get_box_args(time_descript_elements)
  return time_descript_elements_args
end

//takes a json description and draws the weather elements in the right boxes based on times
function draw_weather with description do
  set target_date to ["2025", "02", "25"]
  set time to {"time1": "06:00", "time2": "07:00", "time3": "08:00", "time4": "09:00"}
  set weather_map to json_parser(description, time, target_date)

  draw_sky()
  for each item in weather_map do
    for each element in item["elements"] do
      if item["description"] == "sunny" do
        draw_sunshine(item["args"])
      else if element == "sun" do
        draw_sun(item["args"])
      else if element == "cloud" do
        draw_cloud(item["args"])
      else if element == "rain" do
        draw_rain(item["args"])
      else if element == "snow" do
        draw_snow(item["args"])
      end
    end
  end
end