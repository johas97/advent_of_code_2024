cmake_minimum_required(VERSION 3.27)


# Define a function to compute absolute value
function(abs result number)
    if(${number} LESS 0)
        math(EXPR abs_value "-1 * ${number}")
    else()
        set(abs_value ${number})
    endif()
    set(${result} ${abs_value} PARENT_SCOPE)
endfunction()

###Main
project(1_dec_cmake)
file(READ "input.txt" input)

#Add CMake seperator. AKA make it a list 
string(REGEX REPLACE "   " ";" input "${input}")
string(REGEX REPLACE "\n" ";" input "${input}")

#Split into 2 Lists 
set(LEFT_COLUMN ON)
set(ID_LEFT)
set(ID_RIGHT)

foreach(ID IN LISTS input)

    if(LEFT_COLUMN)
        list(APPEND ID_LEFT "${ID}")
        set(LEFT_COLUMN OFF)

    else()
        list(APPEND ID_RIGHT "${ID}")
        set(LEFT_COLUMN ON)

    endif()   
endforeach(ID IN LISTS input)

#Sort the lists 
list(SORT ID_LEFT COMPARE NATURAL)
list(SORT ID_RIGHT COMPARE NATURAL)

#Get length of list
list(LENGTH ID_LEFT LIST_LENGTH)
math(EXPR LIST_LENGTH2 "${LIST_LENGTH} - 1")

#Compare sorted elements & add together
set(SUM_DIST 0)
foreach(i RANGE 0 ${LIST_LENGTH2})

    #Get elment by idnex
    list(GET ID_LEFT ${i} SINGLE_ID_L)
    list(GET ID_RIGHT ${i} SINGLE_ID_R)

    math(EXPR DIFF "${SINGLE_ID_R} - ${SINGLE_ID_L}")
    abs(ABS_DIFF ${DIFF})
    math(EXPR SUM_DIST "${SUM_DIST} + ${ABS_DIFF}")
endforeach()

#Print result 
message("${SUM_DIST}")





