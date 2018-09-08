# subnet-calculator
##Application 01 ####
import random
import sys


def Subnet_Calc():
    try:
        print ("\n")

        #IP address input and validity check
        while True:
            ip_address = input("Please Enter IP address: ")

            #check octets
            a = ip_address.split('.')

            if (len(a) == 4) and (1 <= int(a[0]) <= 223) and (int(a[0]) != 127) and (int(a[0]) != 169 or int(a[1]) != 254) and (0 <= int(a[1]) <= 255 and 0 <= int(a[2]) <= 255 and 0 <= int(a[3]) <= 255):
                break
            else:
                print("\nThe IP(",ip_address,")address is INVVALID! Please retry!\n")
                continue
        masks = [255,254,252,248,240,224,192,128,0]


        # Subnet mask input and validity check

        while True:
            subnet_mask = input("Please Enter subnet mask: ")

            # check octets
            b = subnet_mask.split('.')

            if(len(b) == 4) and (int(b[0]) == 255) and (int(b[1]) in masks) and (int(b[2]) in masks) and (int(b[3]) in masks) and (int(b[0]) >= int(b[1]) >= int(b[2]) >= int(b[3])):
               break
            else:
                 print("\nThe subnet mask(", subnet_mask, ")is INVVALID! Please retry!\n")
                 continue



         #Convert the mask to binary

        mask_octets_padded=[]

        mask_octets_decimal = subnet_mask.split('.')

        print(mask_octets_decimal)

        for octet_index in range(0,len(mask_octets_decimal)):
            #print (bin(int(mask_octets_decimal[octet_index])))
            binary_octet = bin(int(mask_octets_decimal[octet_index])).split("b")[1]
            #print( binary_octet)

            if len(binary_octet) == 8:
                mask_octets_padded.append(binary_octet)

            elif len(binary_octet) < 8:
                binary_octet_padded = binary_octet.zfill(8)
                mask_octets_padded.append(binary_octet_padded)
        print (mask_octets_padded)

        decimal_mask = "".join(mask_octets_padded)
        print (decimal_mask)

        # Counting host bits in the mask and calculating number of hosts/subnet
        no_of_zeros = decimal_mask.count("0")
        no_of_ones = 32 - no_of_zeros
        no_of_hosts = abs(2 ** no_of_zeros - 2)  # return positive value for mask /32

        print(no_of_zeros)
        print(no_of_ones)
        print(no_of_hosts)

        # Obtaining wildcard mask
        wildcard_octets = []
        for w_octet in mask_octets_decimal:
            wild_octet = 255 - int(w_octet)
            wildcard_octets.append(str(wild_octet))

        print (wildcard_octets)

        wildcard_mask = ".".join(wildcard_octets)
        print (wildcard_mask)

    except KeyboardInterrupt:
        print ("\n\nProgram aborted by user. Exiting..\n")
        sys.exit()


Subnet_Calc()

