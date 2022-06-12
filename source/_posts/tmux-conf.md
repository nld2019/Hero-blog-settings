---
title: tmux.conf
date: 2022-06-12 15:07:59
tags: Misc
---

// # ~/.tmux.conf
// 
// # unbind default prefix and set it to ctrl-a
// unbind C-b
// set -g prefix C-a
// bind C-a send-prefix
// 
// # make delay shorter
// set -sg escape-time 0
// 
// 
// 
// #### key bindings ####
// 
// # reload config file
// bind r source-file ~/.tmux.conf \; display ".tmux.conf reloaded!"
// 
// # quickly open a new window
// bind N new-window
// 
// # synchronize all panes in a window
// bind y setw synchronize-panes
// 
// # pane movement shortcuts (same as vim)
// bind h select-pane -L
// bind j select-pane -D
// bind k select-pane -U
// bind l select-pane -R
// 
// #### copy mode : vim ####
// 
// # set vi mode for copy mode
// setw -g mode-keys vi
// 
// # copy mode using 'Esc'
// unbind [
// bind Escape copy-mode
// 
// # select and copy
// bind-key -T copy-mode-vi v send-keys -X begin-selection
// bind-key -T copy-mode-vi y send-keys -X rectangle-toggle
// 
// # paste using 'p'
// unbind p
// bind p paste-buffer
