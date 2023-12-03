#!/bin/bash


generate_card_number() {
		# Step 1: Random ตัวเลขอะไรมาก็ได้จำนวน 12 ตัว โดยมีกฎอยู่ว่า ตัวแรกห้ามเป็นเลข 0 หรือ 9 เช่น 189353534573
    random_number=$(shuf -i 200000000000-899999999999 -n 1)

    # Step 2: เอาเลข 12 หลักจากข้อแรกมาคูณกับเลข n - index ของมัน แล้วเอามารวมกัน เช่นจากตัวอย่างก็คือ (1*13) + (8*12) + (9*11) + (3*10) + (5*9) + (3*8) + (5*7) + (3*6) + (4*5) + (5*4) + (7*3) + (3*2) ได้ 427
    sum=0
    for ((i=0; i<12; i++)); do
        digit=${random_number:i:1}
        position=$((13 - i))
        product=$((digit * position))
        sum=$((sum + product))
    done

		# Step 3: นำผลลัพธ์จาก step2 มา mod หรือหารเอาเศษด้วย 11
    remainder=$((sum % 11))

		# Step 4: นำ 11 ไปลบกับผลลัพธ์จาก stepที่ 3
    thirteenth_digit=$((11 - remainder))

		# Step 5: นำผลลัพธ์จาก step4มาต่อกับ step1
    echo "${random_number}${thirteenth_digit}"
}

# function สำหรับแสดง help
display_help() {
    echo "Usage: $0 cardId [OPTIONS]"
    echo "Generate Thai id"

    echo "Options:"
    echo "  -n, --num-cards NUM    Specify the number of card id to generate (default is 1)."
    echo "  -f, --file FILE        Specify the file to save the card id"
    echo "  -h, --help             Display this help message."
}

# Add Command cardId
# if arguments $1 เท่ากับ command name 'cardId'
if [ "$1" == "cardId" ]; then

		# check arguments $2 เท่ากับ -h|--help
		case "$2" in
        -h|--help)
            display_help
            exit 0
            ;;
    esac
    num_cards=1  # Default ให้ generating 1 card ถ้าไม่ระบุ -n option
    file_path="" # Default ให้ path ไม่มีค่า หรือ ไม่ต้อง write file

		# Loop while หา command-line arguments 
    while [[ $# -gt 0 ]]; do
        case "$1" in
						# check arguments เท่ากับ option -n --num-cards
            -n|--num-cards)
                num_cards="$2"
								# shift ไป arguments ถัดไป
                shift 
                ;;
						# check arguments เท่ากับ option -f|--file
            -f|--file)
                file_path="$2"
								# shift ไป arguments ถัดไป
                shift 
                ;;
            *)
                shift
                ;;
        esac
    done

    # Generate และ display ตามจำนวนของค่า -n หรือ num_cards
    for ((i=0; i<num_cards; i++)); do
        card_id=$(generate_card_number)
        echo "Generated Card Number $((i+1)): $card_id"

        # Check file_path ถ้ามีค่าทำ echo
        if [ -n "$file_path" ]; then
						# echo หรือ write ข้อมูลลง file
            echo "$card_id" >> "$file_path"
        fi
    done
else
    echo "Invalid command."
    display_help
fi