import sys
import time

def text2word(filename):
    with open(filename) as f:
        word_list = [i for i in f.read().split()]
    return word_list

#########################
# levenschtein distance #
#########################
def levenschtein(source, target):
    source = ' '+source
    target = ' '+target
    n = len(source)
    m = len(target)

    # create a distance matrix
    row = [0 for i in range(m)]
    d = []
    for i in range(n):
        d.append(row[::])

    # initialize the matrix
    d[0] = [i for i in range(m)]
    for i in range(n):
        d[i][0] = i

    # recurrence relation
    for i in range(1,n):
        for j in range(1,m):
            up = d[i-1][j]
            left = d[i][j-1]
            up_left = d[i-1][j-1]
            if source[i] == target[j]:
                cost=0
            else:
                cost=1
            d[i][j] = min(up + 1, left + 1, up_left+cost)
    return d[n-1][m-1]


##################
# osa distance 2 #
##################

def osa(source, target):
    source = ' '+source
    target = ' '+target
    n = len(source)
    m = len(target)

    # create a distance matrix
    row = [0 for i in range(m)]
    d = []
    for i in range(n):
        d.append(row[::])

    # initialize the matrix
    d[0] = [i for i in range(m)]
    for i in range(n):
        d[i][0] = i

    # recurrence relation
    for i in range(1,n):
        for j in range(1,m):
            up = d[i-1][j]
            left = d[i][j-1]
            up_left = d[i-1][j-1]
            if source[i] == target[j]:
                cost=0
            else:
                cost=1
            d[i][j] = min(up + 1, left + 1, up_left+cost)
            if i>1 and i>1 and source[i]==target[j-1] and source[
                i-1]==target[j]:
                d[i][j] = min(d[i][j], d[i-2][j-2]+cost)
    return d[n-1][m-1]

####################
# damerau distance #
####################
def damerau(source, target):
    n = len(source)+2
    m = len(target)+2

    # a array of 24 zeros
    letters = 'abcdefghijklmnopqrstuvwxyz'
    count = [0 for i in range(24)]
    da = {k:v for k,v in zip(letters, count)}

    # create a distance matrix
    row = [0 for i in range(m)]
    d = []
    for i in range(n):
        d.append(row[::])
    maxdist = len(source) + len(target)

    # initialize the matrix
    d[-1][-1] = maxdist
    for i in range(len(source)+1):
        d[i][-1] = maxdist
        d[i][0] = i
    for j in range(len(source)+1):
        d[-1][j] = maxdist
        d[0][j] = j

    # recurrence relation
    for i in range(1,len(source)+1):
        db = 0
        for j in range(1, len(target)+1):
            k = int(da[target[j-1]])
            l = db
            if source[i-1] == target[j-1]:
                cost = 0
                db = j
            else:
                cost = 1

            up = d[i-1][j]
            left = d[i][j-1]
            up_left = d[i-1][j-1]
            d[i][j] = min(up+1, left+1, up_left+cost,
                         d[k-1][l-1] + (i-k-1) + 1 + (j-l-1))
        da[source[i-1]] = i
    return d[len(source)][len(target)]

print(damerau('ca','abc')) # should be 2


# input_word is a list of possibly misspelled words
# right_word is a list of correct words

def correct_words(input_word, right_word, mode):
    result = []
    for i in input_word:
        best_dist = 1000000
        best_word = None
        if i in right_word: result.append([i,i,0])
        else:
            for j in right_word:
                if mode=='1':
                    dist = levenschtein(i, j)
                elif mode=='2':
                    dist = osa(i, j)
                elif mode=='3':
                    dist = damerau(i, j)
                if dist < best_dist:
                    best_dist = dist
                    best_word = j
            result.append([i, best_word, best_dist])
    return result



def correct_words1(input_word, right_word, mode):
    result = []
    if mode=='1':
        for i in input_word:
            best_dist = 1000000
            best_word = None
            for j in right_word:
                dist = levenschtein(i,j)
                if dist<best_dist:
                    best_dist=dist
                    best_word=j
            result.append([i, best_word, best_dist])

    elif mode=='2':
        for i in input_word:
            best_dist = 1000000
            best_word = None
            for j in right_word:
                dist = osa(i,j)
                if dist<best_dist:
                    best_dist=dist
                    best_word=j
            result.append([i, best_word, best_dist])

    elif mode=='3':
        for i in input_word:
            best_dist = 1000000
            best_word = None
            for j in right_word:
                dist = damerau(i,j)
                if dist<best_dist:
                    best_dist=dist
                    best_word=j
            result.append([i, best_word, best_dist])
    return result

def main():
    mode = sys.argv[1]
    input = sys.argv[2]
    word_dict = sys.argv[3]
    output = sys.argv[4]
    input_word = text2word(input)
    right_word = text2word(word_dict)

    ##########
    # Task 1 #
    ##########
    if mode == '1':
        start = time.time()
        result = correct_words(input_word, right_word, mode)
        end = time.time()
        duration = end - start

    ##########
    # Task 2 #
    ##########
    elif mode == '2':
        start = time.time()
        result = correct_words(input_word, right_word, mode)
        end = time.time()
        duration = end - start

    # write out to a file
    with open(output,'w') as f:
        for i in result:
            f.write("{} {} {}".format(i[1], i[2], '\n'))
        #f.write(str(duration))


if __name__ == '__main__':
    main()
