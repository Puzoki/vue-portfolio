<template>
    <div class="colors-container">
        <div class="col">
            <span>hex</span>
            <div class="icon" data-type="notJoined">
                <svg 
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 418 512">
                    <path :d="this.notJoined"/>
                </svg>
            </div>
        </div>
        <div class="col">
            <span>hex</span>
            <div class="icon" data-type="notJoined">
                <svg 
                xmlns="http://www.w3.org/2000/svg" 
                viewBox="0 0 418 512">
                    <path :d="this.notJoined"/>
                </svg>
            </div>
        </div>
        <div class="col">
            <span>hex</span>
            <div class="icon" data-type="notJoined">
                <svg 
                xmlns="http://www.w3.org/2000/svg" 
                viewBox="0 0 418 512">
                    <path :d="this.notJoined"/>
                </svg>
            </div>
        </div>
        <div class="col">
            <span>hex</span>
            <div class="icon" data-type="notJoined">
                <svg 
                xmlns="http://www.w3.org/2000/svg" 
                viewBox="0 0 418 512">
                    <path :d="this.notJoined"/>
                </svg>
            </div>
        </div>
        <div class="col">
            <span>hex</span>
            <div class="icon" data-type="notJoined">
                <svg 
                xmlns="http://www.w3.org/2000/svg" 
                viewBox="0 0 418 512">
                    <path :d="this.notJoined"/>
                </svg>
            </div>
        </div>
    </div>
    <div class="col-2 alert alert-success mt-3 mx-auto text-center" role="alert" id="alert">hex-код скопирован!</div>
</template>

<script> 
    import chroma from "chroma-js"

    export default {
        name: "randomColors",
        data() {
            return {
                isJoined: 'M416 256c0 17.7-14.3 32-32 32L32 288c-17.7 0-32-14.3-32-32s14.3-32 32-32l352 0c17.7 0 32 14.3 32 32z',
                notJoined: 'M240 80c0-17.7-14.3-32-32-32s-32 14.3-32 32V224H32c-17.7 0-32 14.3-32 32s14.3 32 32 32H176V432c0 17.7 14.3 32 32 32s32-14.3 32-32V288H384c17.7 0 32-14.3 32-32s-14.3-32-32-32H240V80z',
            }
        },
        mounted() {
            const icons = document.querySelectorAll('.icon')
            const spans = document.querySelectorAll('span')

            icons.forEach((icon) => {
                icon.addEventListener('click', this.changeSvg)
            })
            document.addEventListener('keydown', (e) => {
                if (e.code === 'Enter') {this.setRandomColor()}
            })
            spans.forEach((span) => {
                span.addEventListener('click', this.copyToClipboard)
            })
            
            this.setRandomColor(true)
        },
        methods: {
            setRandomColor(isInitial) {
                const cols = document.querySelectorAll('.col')
                const colorsArr = isInitial ? this.getColorsFromHash() : []

                cols.forEach((col, index) => {
                    const isJoined = col.querySelector('.icon').dataset.type
                    const span = col.querySelector('span')
                    const icon = col.querySelector('svg')
                    
                    if (isJoined === 'isJoined') {
                        colorsArr.push(span.textContent)
                        return
                    }

                    const color = isInitial
                        ? colorsArr[index]
                            ? colorsArr[index]
                            : chroma.random()
                        : chroma.random()

                    if (!isInitial) {
                        colorsArr.push(color)
                    }

                    span.textContent = color
                    col.style.background = color
                    this.setElementsColor(span, icon, color)
                })

                this.updateHash(colorsArr)
            },
            setElementsColor(span, icon, color) {
                const luma = chroma(color).luminance()

                span.style.color = luma > 0.5 ? 'black' : 'white'
                icon.style.fill = span.style.color
            },
            changeSvg() {
                if (event.currentTarget.dataset.type === 'notJoined') {
                    event.currentTarget.dataset.type = 'isJoined'
                    event.currentTarget.querySelector('path').setAttribute('d', this.isJoined)
                } else {
                    event.currentTarget.dataset.type = 'notJoined'
                    event.currentTarget.querySelector('path').setAttribute('d', this.notJoined)
                }
            },
            copyToClipboard() {
                this.alert()
                return navigator.clipboard.writeText(event.target.textContent)
            },
            updateHash(colors=[]) {
                document.location.hash = colors.map(color => color.toString().substring(1)).join('-')
            },
            getColorsFromHash() {
                if (document.location.hash.length > 1) {
                    return document.location.hash
                    .substring(1)
                    .split('-')
                    .map((color) => '#' + color)
                }
                return []
            },
            alert() {
                document.getElementById('alert').style.visibility="visible"
                setTimeout(() => {
                    document.getElementById('alert').style.visibility="hidden"
                }, 1000);
            },
        },
        destroyed() {
            const locks = document.querySelectorAll('.icon')
            const spans = document.querySelectorAll('span')

            locks.forEach((lock) => {
                lock.removeEventListener('click', this.changeSvg)
            })
            document.removeEventListener('keydown', (e) => {
                if (e.code === 'Enter') {this.setRandomColor()}
            })
            spans.forEach((span) => {
                span.removeEventListener('click', this.copyToClipboard)
            })
        }
    } 
</script>

<style scoped>
    .colors-container {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        display: flex;
        width: 60vw;
        height: 60vh;
    }

    .col {
        flex: 1;
        display: flex;
        flex-direction: column;
        justify-content: space-around;
        align-items: center;
        background-color: cadetblue;
    }

    .col span {
        font-size: 1.5rem;
        font-weight: bold;
        border-radius: 0.75rem;
        padding: 0.75rem;
        transition: background .2s;
    }

    .col span:hover {
        background: rgba(0,0,0,0.1);
        cursor: pointer;
    }

    .icon {
        background: transparent;
        border: none;
        width: 30px;
        height: 30px;
        padding: 0.75rem;
        border-radius: 0.75rem;
        box-sizing: content-box;
        transition: background .2s;
    }

    .icon:hover {
        background: rgba(0,0,0,0.1);
        cursor: pointer;
    }

    svg {
        position: relative;
        top: -2px;
    }

    .alert {
        visibility: hidden;
        transition: visibility .5s;
    }
</style>