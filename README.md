# -*- coding: utf-8 -*-
"""
Created on Tue May 10 10:33:32 2022
Credits: Tutorial by federicocotogno
@author: janelle
"""


#Credits: Followed Tutorial from https://github.com/federicocotogno/te...
# Customer-Service-Bot
#Potential Broil King Customer Service Bot
import re

import long_responses as long
#ca
def message_probability(user_message, recognized_words, single_resp=False, req_words=()):
    message_certainty = 0
    has_req_words = True
    
    #counts how many words are in each message (would this mean how many )
    for word in user_message:
        if word in recognized_words:
            message_certainty += 1
            
    # dividing total amount of words which will give the percent of words recognized
    percentage = float(message_certainty) / float(len(recognized_words))
    
    #avoids sending the wrong message if the recognized words are not in the sentence
    for word in req_words:
        if word not in user_message:
            has_req_words = False 
            break
    
    if has_req_words or single_resp:
        return int(percentage*100)
    else:
        return 0
    
def check_all_messages(message):
    highest_prob_list = {}
    
    

import random


#long repsosnes-----------------------------------------------------------------------------------------------------------------------
R_CASH= 'To make a purchase please have your model number handy, and call the 1-800-265-2150. Thank you for chosing Onward!' 
R_EXT_WARR= 'To verify your warranty please make sure you have your model number, serial number, and date of purchase handy. You can give the 1-800-265-2150 a call and they will assist you with your order!'
R_HUMAN= 'No problem, you can talk to one of our customer service representatives by calling the 1-800-265-2150! Have a great day!'
R_CONV= 'We do not offer conversion kits and simply sell the replacement parts that would be needed for the conversion. We do not recommend this action be taken. Please note our BBQs are CSA certified to work the way they were manufactured and converting the BBQ would void any warranaty related to gas components. For any questions, you can email info@omcbbq.com or call the 1-800-265-2150.'


def unknown():
    response = ['Sorry I am afraid I did not understand that?', 
                'Could you please rephrase that?', 
                'I am afraid I can not help you, please call the 1-800-265-2150 for assistance!'][random.randrange(3)]
    return response
    
    

    def response(bot_resp, list_of_words, single_resp=False, req_words=[]):
        nonlocal highest_prob_list
        highest_prob_list[bot_resp] = message_probability(message, list_of_words, single_resp, req_words)
    #response---------------------------------------------------------------------------------------------------------
    response('Hello, I am a customer service bot. What can I help you with today?', ['hello', 'hi', 'good morning' ], single_resp=True)
    response('My pleasure, thank you for chatting with Onward!', [ 'thank', 'you', 'helpful', 'nice', 'ok' ], single_resp=True)
    response('Sadly, we do not sell BBQs, we only deal wiht warranty and purchases here!', ['buy', 'purchase','bbq', 'get' ], req_words= ['buy'])
    response('Would this item be under warranty or a purchase item?', ['parts', 'burners', 'flav r waves', 'grids', 'lid', 'wheels'], single_resp=True)
    response('I am afraid our warranty policy does not include returns.', ['refund','refunds', 'new', 'get', 'BBQ', 'give'], single_resp=True)
    response(long.R_CONV, ['conversion', 'natural gas', 'propane', 'from', 'kit'], req_words=['conversion'])
    response(long.R_HUMAN, ['human', 'someone', 'speak', 'person'], single_resp=True)
    response(long.R_EXT_WARR, ['warranty', 'have', 'check'], req_words=[ 'warranty'])   
    response(long.R_CASH, ['buy', 'purchase', 'parts'], single_resp=True)
    
    
    
    best_match = max(highest_prob_list, key=highest_prob_list.get)
    #print(highest_prob_list)
    #case check for when user inputs value that dont match any probability
    return long.unknown() if highest_prob_list[best_match] < 1 else best_match
    #return best_match
    
    
    
    
def get_response(user_input):
    # re.split takes out the characters form messages so that the bot can 
    #focus on questions makes it much cleaner to recognize
    #user_input.lower insures that response is taken in lower case since 
    #python is case sensitive
    split_message = re.split(r'\s+|[,;?!.-]\s*', user_input.lower())
    response = check_all_messages(split_message)
    return response

#Testing out a response system
while True: 
     print('Bot:'+ get_response(input('you:')))
