# 미니쉘 시작하기
히히 쉘 이름 띄우고, exit 오면 끄기

```C
/* ************************************************************************** */
/*                                                                            */
/*                                                        :::      ::::::::   */
/*   start_shell.c                                      :+:      :+:    :+:   */
/*                                                    +:+ +:+         +:+     */
/*   By: yeosong <yeosong@student.42.fr>            +#+  +:+       +#+        */
/*                                                +#+#+#+#+#+   +#+           */
/*   Created: 2020/11/20 19:13:09 by yeosong           #+#    #+#             */
/*   Updated: 2020/11/20 22:23:11 by yeosong          ###   ########.fr       */
/*                                                                            */
/* ************************************************************************** */

#include "minishell.h"
#include <string.h>

int		command(char *line)
{
	if (!strcmp(line,"exit"))
		exit(0);


	return (0);
}

int		main(void)
{
	int		r;
	char	*line;

	line = NULL;
	while (ft_printf(GR_SH)  && (r = get_next_line(&line)) > 0)
	{
		if (!command(line) && !!strcmp(line,"\0"))
		{
			ft_printf(GR_SH);
			ft_printf("%s\n", line);
		}
		free(line);
		line=NULL;
	}
}
```
