# **************************************************************************** #
#                                                                              #
#                                                         :::      ::::::::    #
#    Makefile                                           :+:      :+:    :+:    #
#                                                     +:+ +:+         +:+      #
#    By: gguiulfo <gguiulfo@student.42.fr>          +#+  +:+       +#+         #
#                                                 +#+#+#+#+#+   +#+            #
#    Created: 2017/06/20 11:36:52 by gguiulfo          #+#    #+#              #
#    Updated: 2018/01/29 00:20:05 by gguiulfo         ###   ########.fr        #
#                                                                              #
# **************************************************************************** #

NAME		:= ft_tar

DEBUG		:= 0

CC			?= gcc
CFLAGS		+= -std=c99
CFLAGS		+= -Wall -Wextra -Werror
CFLAGS		+= -DDEBUG=${DEBUG}
# CFLAGS		+= -Wall -Wextra -Werror -Wfloat-equal -Wundef
# CFLAGS		+= -Wpointer-arith -Wunreachable-code -Winit-self
# CFLAGS		+= -O3 -march=native -pipe -flto

ifeq ($(DEBUG),1)
CFLAGS		+= -g -fsanitize=address
endif

ifndef VERBOSE
MAKEFLAGS	+= --no-print-directory
endif

SRC_BASE	:=	\
ft_archive\
ft_tar\
ft_unarchive\
unutils\
utils\

INC_DIR		:= includes/
OBJ_DIR		:= obj/
SRC_DIR		:= src/
LIBFT_DIR	:= ../libft/
LIBFT_INC	:= $(LIBFT_DIR)includes/
LIBFT_LIB	:= $(LIBFT_DIR)libft.a
LIBFT_LNK	:= -L libft/ -lft
SRC			:= $(addprefix $(SRC_DIR), $(addsuffix .c, $(SRC_BASE)))
OBJ			:= $(patsubst $(SRC_DIR)%, $(OBJ_DIR)%, $(SRC:.c=.o))
INC_BASE	:= $(wildcard $(INC_DIR)*.h)
SHELL		:= /bin/bash
BASENAME  	:= `basename -s .a $(NAME)`
LEN_NAME	:= `printf "%s" $(NAME) | wc -c`
DELTA		:= $$(echo "$$(tput cols)-31-$(LEN_NAME)" | bc)
NB			:= $(words $(SRC_BASE))
INDEX		:= 0

all:
	@$(MAKE) -C $(LIBFT_DIR)
	@$(MAKE) -j $(NAME)

$(NAME):	$(LIBFT_LIB) $(OBJ_DIR) $(OBJ)
	@$(CC) $(OBJ) -o $@\
			-I $(INC_DIR) \
			-I $(LIBFT_INC) \
			$(LIBFT_LIB)\
			$(CFLAGS)
	@printf "\r\033[38;5;15m✓ make   $(BASENAME)\033[0m\033[K\n";

$(OBJ_DIR):
	@mkdir -p $(OBJ_DIR)
	@mkdir -p $(dir $(OBJ))

$(OBJ_DIR)%.o:$(SRC_DIR)%.c Makefile | $(OBJ_DIR)
	@$(eval DONE=$(shell echo $$(($(INDEX)*20/$(NB)))))
	@$(eval PERCENT=$(shell echo $$(($(INDEX)*100/$(NB)))))
	@$(eval TO_DO=$(shell echo $$((20-$(INDEX)*20/$(NB) - 1))))
	@$(eval COLOR=$(shell list=(141 105 69 33 39 45 51 87 123 159 195 231 15); index=$$(($(PERCENT) * $${#list[@]} / 100)); echo "$${list[$$index]}"))
	@printf "\r\033[38;5;%dm⌛ [%s]:%2d%% `printf '█%.0s' {0..$(DONE)}`%*s❙ %*.*s\033[0m\033[K" $(COLOR) $(NAME) $(PERCENT) $(TO_DO) "" $(DELTA) $(DELTA) "$(shell echo "$@" | sed 's/^.*\///')"
	@$(CC) $(CFLAGS) -MMD -c $< -o $@\
		-I $(INC_DIR)\
		-I $(LIBFT_INC)
	@$(eval INDEX=$(shell echo $$(($(INDEX)+1))))

clean:		cleanlib
	@if [ -e $(OBJ_DIR) ]; \
	then \
		rm -rf $(OBJ_DIR); \
		make -C libft clean; \
		printf "\r\033[38;5;219m✗ clean  $(BASENAME)\033[0m\033[K\n"; \
	fi;

fclean:		clean fcleanlib
	@if [ -e $(NAME) ]; \
	then \
		rm -rf $(NAME); \
		make -C libft fclean; \
		printf "\r\033[38;5;33m✗ fclean $(BASENAME)\033[0m\033[K\n"; \
	fi;

re:			fclean all

libft:		$(LIBFT_LIB)

$(LIBFT_LIB):
	@make -j -C $(LIBFT_DIR)

cleanlib:
	@make -C $(LIBFT_DIR) clean

fcleanlib:	cleanlib
	@make -C $(LIBFT_DIR) fclean

relib:		fcleanlib libft

.PHONY: all clean cleanlib fclean fcleanlib re relib

-include $(OBJ:.o=.d)
