<template>
    <div>
        <div class="tile is-ancestor">
            <div class="tile is-parent">
                <article class="tile is-child">
                    <!-- <p class="title">Tall tile</p>
                        <p class="subtitle">With even more content</p> -->
                    <div class="content">
                        <nav class="panel">
                            <p class="panel-heading">
                                Repositories
                            </p>
                            <div class="panel-block">
                                <p class="control has-icons-left">
                                    <input class="input" type="text" placeholder="Search">
                                    <span class="icon is-left">
                                        <i class="fas fa-search" aria-hidden="true"></i>
                                    </span>
                                </p>
                            </div>
                            <p class="panel-tabs">
                                <a class="is-active">All</a>
                                <a>Public</a>
                                <a>Private</a>
                                <a>Sources</a>
                                <a>Forks</a>
                            </p>
                            <a
                                v-for="file in state.files"
                                :key="file.id"
                                class="panel-block is-active"
                            >
                                <span class="panel-icon">
                                    <i class="fas fa-book" aria-hidden="true"></i>
                                </span>
                                {{file.name}}
                            </a>
                            <label class="panel-block">
                                <input type="checkbox">
                                remember me
                            </label>
                            <div class="panel-block">
                                <button class="button is-link is-outlined is-fullwidth">
                                    Reset all filters
                                </button>
                            </div>
                        </nav>
                    </div>
                </article>
            </div>
            <div class="tile is-vertical is-8">
                <div class="tile">
                    <div class="tile is-parent">
                        <article class="tile is-child notification is-info">
                            <div class="content">
                                <button
                                    class="button"
                                    @click="openFolder"
                                >
                                    Open Folder...
                                </button>
                            </div>
                        </article>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script lang="ts">
    import { defineComponent, reactive } from 'vue';
    import * as fs from 'tauri/api/fs';
    import * as dialog from 'tauri/api/dialog';
    import Walker from 'walker';

    export default defineComponent({
        name: 'App',
        setup () {
            const state = reactive({
                files: []
            });

            fs.readDir('/').then((files) => {
                state.files = files;
            });

            function openFolder () {
                dialog.open({ directory: true })
                    .then((res) => {
                        Walker(res)
                            .on('entry', function (entry, stat) {
                                console.log('Got entry: ' + entry);
                            })
                            .on('dir', function (dir, stat) {
                                console.log('Got directory: ' + dir);
                            })
                            .on('file', function (file, stat) {
                                console.log('Got file: ' + file);
                            })
                            .on('symlink', function (symlink, stat) {
                                console.log('Got symlink: ' + symlink);
                            })
                            .on('blockDevice', function (blockDevice, stat) {
                                console.log('Got blockDevice: ' + blockDevice);
                            })
                            .on('fifo', function (fifo, stat) {
                                console.log('Got fifo: ' + fifo);
                            })
                            .on('socket', function (socket, stat) {
                                console.log('Got socket: ' + socket);
                            })
                            .on('characterDevice', function (characterDevice, stat) {
                                console.log('Got characterDevice: ' + characterDevice);
                            })
                            .on('error', function (er, entry, stat) {
                                console.log('Got error ' + er + ' on entry ' + entry);
                            })
                            .on('end', function () {
                                console.log('All files traversed.');
                            });
                    });
                // .then(fs.readDir)
                // .then((files) => {
                //     state.files = files;
                // });
            }

            return {
                state,
                openFolder
            };
        }
    });
</script>
