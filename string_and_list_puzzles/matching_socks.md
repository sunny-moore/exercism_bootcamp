//functions in this set
//length(string_or_list)
//sort_out_socks(list)
//is_sock(name_as_list)
//is_matching_sock(sock_1, sock_2)
//string_to_list(string)
//create_sock_match(sock)
//remove_duplicates(string_list)
//is_duplicate(list, string)
//has_space(string)
//
//takes two lists of string items and returns a list of matching socks if present
function matching_socks with clean_basket_list, dirty_basket_list do
  set whole_basket to concat(clean_basket_list, dirty_basket_list)
  change whole_basket to remove_duplicates(whole_basket)
  set sock_basket_list to sort_out_socks(whole_basket)
  set matched_socks to []
  set outer_index to 1
  set inner_index to 2
  set total_items to length(sock_basket_list)
  if total_items <=1 do
    return [] //one or none socks, no match
  end

  for each sock1 in sock_basket_list do
      for each sock2 in sock_basket_list do
        if inner_index <= total_items and is_matching_sock(sock1, sock_basket_list[inner_index]) do 
          change matched_socks to push(matched_socks, create_sock_match(sock1))
        end
        change inner_index to inner_index + 1
      end
    change outer_index to outer_index + 1
    change inner_index to outer_index + 1
  end
  return matched_socks
end

//takes a list of strings and return a nested list of only socks
function sort_out_socks with basket_list do
  set sock_basket to []
  set list_item to []
  for each item in basket_list do
    change list_item to string_to_list(item)
    log(list_item)
      if is_sock(list_item) do
        change sock_basket to push(sock_basket, list_item)
      end
  end
  return sock_basket
end

//returns true if the item is a sock
function is_sock with item_name_as_list do
  set index to length(item_name_as_list)
  if index > 0 do
    return item_name_as_list[index] == "sock"
  else if index == 0 do
    return false
  end
end

function is_matching_sock with sock_list1, sock_list2 do
  if (sock_list1[1] == "left" and sock_list2[1] == "right") or (sock_list1[1] == "right" and sock_list2[1] == "left") do
    return sock_list1[2] == sock_list2[2]
  else do
    return false
  end
end

//takes a string and splits it into a list at the spaces
//or if there are no spaces return the string as a list of one element
function string_to_list with string do
  set new_list to []
  set last_index to length(string) //should work on lists and strings
  if has_spaces(string) do
    set current_word to ""
    set index to 1
    for each char in string do
      if char != " " do
        change current_word to join(current_word, char)
      end
      if index == last_index or string[(index + 1)] == " "  do
        change new_list to push(new_list, current_word)
        change current_word to ""
      end
      change index to index + 1
    end    
  else do 
    //entered if string has no spaces
    push(new_list, string)
  end
  return new_list
end

//don't need to pass two socks in because at this point we know its a match
function create_sock_match with sock_as_list do
  set sock_match to join(sock_as_list[2], " socks")
  return sock_match
end

//takes a list of strings and returns a new list with duplicates removed
function remove_duplicates with string_list do
  set new_list to []
  for each string in string_list do
    if is_duplicate(new_list, string) == false do
      change new_list to push(new_list, string)
    end
  end
  return new_list
end

//returns true if the string appears more than once in a list of strings
function is_duplicate with a_list, string do
  set is_duplicate to false
  for each item in a_list do
    if item == string do
      change is_duplicate to true
    end
  end
  return is_duplicate
end

//return true if there is at least one space
function has_spaces with string do
  for each char in string do
    if char == " " do
      return true
    end
  end
  return false
end

//take a list or string and return how many elements it has
function length with list do 
  set count to 0
  for each item in list do
    change count to count + 1
  end
  return count
end