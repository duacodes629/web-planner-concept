ALGORITHM: Web-Based Study Task Tracker

START
    // Step 1: Initialise empty array to hold tasks
    DECLARE tasks_list AS ARRAY OF OBJECTS

    // Step 2: Main Event Listener for Form Submission
    WHEN user clicks "Add Task" button:
        READ task_name_input
        READ study_hours_input

        // Step 3: Input Validation Algorithm
        IF task_name_input IS EMPTY THEN
            PRINT "Error: Task name cannot be blank."
            DISPLAY red border on input field
            EXIT action
        ENDIF

        TRY
            CONVERT study_hours_input TO NUMBER
            IF study_hours_input <= 0 THEN
                PRINT "Error: Hours must be greater than zero."
                EXIT action
            ENDIF
        CATCH ValueError
            PRINT "Error: Please enter a valid number for hours."
            EXIT action
        END TRY

        // Step 4: Process and Append Valid Data
        CREATE new_task OBJECT WITH task_name_input, study_hours_input
        APPEND new_task TO tasks_list

        // Step 5: Render to Web Interface
        CALL RefreshWebUI()
        CLEAR input fields
END

FUNCTION RefreshWebUI():
    CLEAR current displayed HTML task list
    FOR EACH task IN tasks_list:
        CREATE HTML <li> element
        SET <li> text TO task.name + " (" + task.hours + " hrs)"
        APPEND <li> to HTML <ul> container
    ENDFOR
RETURN
